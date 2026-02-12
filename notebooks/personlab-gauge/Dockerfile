FROM python:3.11-bookworm

ARG DEBIAN_FRONTEND=noninteractive
ARG NB_USER=newuser
ARG NB_UID=1000
ARG NB_GID=1000

RUN apt-get update && apt-get install -y \
    build-essential git unzip curl libgl1 nano wget tree \
    openjdk-17-jdk ffmpeg libsm6 libxext6 \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Python deps first (layer cache)
COPY requirements.txt /tmp/requirements.txt
RUN pip install -U pip && \
    pip install --no-cache-dir -r /tmp/requirements.txt

# --- ensure user and base dirs exist BEFORE touching /home/${NB_USER} ---
RUN groupadd -g ${NB_GID} ${NB_USER} || true \
 && id -u ${NB_UID} >/dev/null 2>&1 || useradd -m -s /bin/bash -u ${NB_UID} -g ${NB_GID} ${NB_USER} \
 && install -d -o ${NB_UID} -g ${NB_GID} /home/${NB_USER} \
 && install -d -o ${NB_UID} -g ${NB_GID} /home/${NB_USER}/.local/bin \
 && install -d -o ${NB_UID} -g ${NB_GID} /home/${NB_USER}/.cache

# Clone + install as root into system site-packages
# RUN --mount=type=cache,target=/root/.cache/git,id=git-cache,sharing=locked \
WORKDIR /home/${NB_USER}
RUN git clone --depth 1 https://github.com/SonySemiconductorSolutions/aitrios-rpi-training-samples \
      /home/${NB_USER}/aitrios-rpi-training-samples \
 && pip install --no-cache-dir --no-deps -e \
      /home/${NB_USER}/aitrios-rpi-training-samples \
 && pip install --no-cache-dir -e \
      /home/${NB_USER}/aitrios-rpi-training-samples/third_party/nanodet/nanodet \
 && chown -R ${NB_UID}:${NB_GID} /home/${NB_USER}

# Bring in notebooks/Makefile owned by the user
COPY --chown=${NB_UID}:${NB_GID} *ipynb Makefile ./

# some magic to be able to correctly use the cuda libs
ENV XLA_FLAGS=--xla_gpu_cuda_data_dir=/usr/local/lib/python3.11/site-packages/triton/third_party/cuda
RUN cd /usr/local/lib/python3.11/site-packages/triton/third_party/cuda && \
  mkdir -p nvvm/libdevice && \
  cp lib/*.bc nvvm/libdevice

# Switch to unprivileged user and set env
USER ${NB_UID}:${NB_GID}
RUN jupyter notebook --generate-config

ENV PYTHONPATH=""
ENV PYTHONPATH="/home/${NB_USER}/.local/lib/python3.11/site-packages:${PYTHONPATH}"
ENV PATH="/home/${NB_USER}/.local/bin:${PATH}"
WORKDIR /home/${NB_USER}
EXPOSE 8888

CMD ["make", "test-github", "TESTDIR=/home/newuser"]
