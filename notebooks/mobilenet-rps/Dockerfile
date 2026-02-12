FROM tensorflow/tensorflow:2.14.0-jupyter

ARG DEBIAN_FRONTEND=noninteractive
# Change these if you want to match host UID/GID
ARG NB_USER=newuser
ARG NB_UID=1000
ARG NB_GID=1000

# System deps (as root)
RUN apt-get update && apt-get install -y \
    libgl1 \
    nano \
    wget \
    tree \
    openjdk-17-jdk \
    ffmpeg libsm6 libxext6 \
 && rm -rf /var/lib/apt/lists/*

# Install Python deps system-wide (as root is fine)
# tf is installed here to prevent imx500-converter to install tf
# tf installation here does not substantially influence layers export (>10min)
RUN pip install --no-cache-dir -U pip \
 && pip install --no-cache-dir \
 tensorflow~=2.14.0 \
 tensorflow_datasets \
 model-compression-toolkit~=2.2.0 \
 imx500-converter[tf]

# Create non-root user
RUN groupadd -g ${NB_GID} ${NB_USER} \
 && useradd -m -s /bin/bash -u ${NB_UID} -g ${NB_GID} ${NB_USER}

# Use a home-owned workspace
WORKDIR /home/${NB_USER}/tutorial
# Ensure files are owned by the non-root user inside the image
COPY --chown=${NB_UID}:${NB_GID} *ipynb Makefile ./
RUN mkdir -p /home/${NB_USER}/tutorial && chown -R ${NB_UID}:${NB_GID} /home/${NB_USER}
 
# Drop privileges for runtime so Jupyter won't be root
USER ${NB_UID}:${NB_GID}
ENV PATH="/home/${NB_USER}/.local/bin:${PATH}"
ENV TESTDIR="/home/${NB_USER}/tutorial"
EXPOSE 8888

# Keep your test default; override at `docker run` to start Jupyter
CMD make test-github TESTDIR=${TESTDIR}