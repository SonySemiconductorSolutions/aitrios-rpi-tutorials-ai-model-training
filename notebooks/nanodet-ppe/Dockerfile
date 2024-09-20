#FROM tensorflow/tensorflow:2.14.0-jupyter
FROM python:3.10-slim

RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    unzip \
    curl \
    libgl1-mesa-glx \
    nano \
    wget \
    tree \
    openjdk-17-jdk \
    ffmpeg libsm6 libxext6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /tmp/tutorial

# - combination of  torch~=1.13.1 torchvision requires that python < 3.11
# - --no-cache-dir: is used because limited amount of memory in github workflow,
#                   without flag -> ERROR: Could not install packages due to an OSError: [Errno 28] No space left on device
RUN pip install -U pip && \
    pip install --no-cache-dir \
        notebook \
        tensorflow~=2.14.0 \
        torch~=1.13.1 \
        torchvision
        
RUN jupyter notebook --generate-config        

COPY *ipynb Makefile ./

CMD make test-github