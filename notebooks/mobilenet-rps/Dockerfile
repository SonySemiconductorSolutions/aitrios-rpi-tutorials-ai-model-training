FROM tensorflow/tensorflow:2.14.0-jupyter

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    libgl1-mesa-glx \
    nano \
    wget \
    tree \
    openjdk-17-jdk \
    ffmpeg libsm6 libxext6 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /tmp/tutorial
COPY *ipynb Makefile ./

RUN pip install -U pip && \
    pip install tensorflow_datasets

CMD make test-github