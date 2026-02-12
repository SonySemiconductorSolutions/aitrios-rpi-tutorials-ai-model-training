# Nanodet training tutorial for IMX500
<!--[![test-nanodet](https://github.com/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training-dev/actions/workflows/test-nanodet.yml/badge.svg)](https://github.com/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training-dev/actions/workflows/test-nanodet.yml)-->

## Dataset
https://universe.roboflow.com/ai-camp-safety-equipment-detection/ppe-detection-using-cv/dataset/3

To use Roboflow open source datasets, you need a Roboflow [public account](https://roboflow.com/pricing) and accept Roboflow [Terms of Service](https://roboflow.com/terms)

## Training and quantization
[custom_nanodet.ipynb](./custom_nanodet.ipynb)

## Running tutorials on Docker
Quantized and converted models created inside the Docker container will appear in the shared folder mapped to `$HOST_WORKDIR` (by default `tutorial/`). See the `Makefile` for customization.

## Tests
See Makefile.