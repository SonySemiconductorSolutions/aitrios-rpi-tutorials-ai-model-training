# Mobilenet training tutorial for IMX500
[![test-mobilenet](https://github.com/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training-dev/actions/workflows/test-mobilenet.yml/badge.svg)](https://github.com/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training-dev/actions/workflows/test-mobilenet.yml)

## Dataset
https://www.tensorflow.org/datasets/catalog/rock_paper_scissors

## Training and quantization
[custom_mobilenet.ipynb](./custom_mobilenet.ipynb)

## Running tutorials on Docker
Quantized and converted models created inside the Docker container will appear in the shared folder mapped to `$HOST_WORKDIR` (by default `tutorial/`). See the `Makefile` for customization.

## Tests
See Makefile.
