#FROM tensorflow/tensorflow:2.14.0-jupyter
FROM python:3.11-bookworm

ARG DEBIAN_FRONTEND=noninteractive
# Change these if you want to match host UID/GID
ARG NB_USER=newuser
ARG NB_UID=1000
ARG NB_GID=1000

RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    unzip \
    curl \
    libgl1 \
    nano \
    wget \
    tree \
    openjdk-17-jdk \
    ffmpeg libsm6 libxext6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# - combination of  torch~=1.13.1 torchvision requires that python < 3.11
# - --no-cache-dir: is used because limited amount of memory in github workflow,
#                   without flag -> ERROR: Could not install packages due to an OSError: [Errno 28] No space left on device
RUN pip install -U pip && \
    pip install --no-cache-dir \
        notebook \
        tensorflow==2.15.1 \
        pycocotools \
        torch~=2.5.0 \
        torchvision \
        imx500-converter[tf] \
        edge-mdt-tpc \
        "numpy<2.0" \
        opencv-python \
        model_compression_toolkit==2.3.0 \
        roboflow

# Create non-root user & workspace
RUN groupadd -g ${NB_GID} ${NB_USER} \
 && useradd -m -s /bin/bash -u ${NB_UID} -g ${NB_GID} ${NB_USER}
WORKDIR /home/${NB_USER}/tutorial
COPY --chown=${NB_UID}:${NB_GID} *ipynb Makefile ./
RUN mkdir -p /home/${NB_USER}/tutorial && chown -R ${NB_UID}:${NB_GID} /home/${NB_USER}

# Generate Jupyter config as the non-root user (so it lands in ~newuser/.jupyter)
USER ${NB_UID}:${NB_GID}
RUN jupyter notebook --generate-config

# Make user + system site-packages visible to the kernel
ENV PYTHONPATH="/home/${NB_USER}/.local/lib/python3.11/site-packages:/usr/local/lib/python3.11/dist-packages:${PYTHONPATH}"

ENV PATH="/home/${NB_USER}/.local/bin:${PATH}"
ENV TESTDIR="/home/${NB_USER}/tutorial"
EXPOSE 8888

# Keep your test default; override at `docker run` to start Jupyter
CMD make test-github TESTDIR=${TESTDIR}
