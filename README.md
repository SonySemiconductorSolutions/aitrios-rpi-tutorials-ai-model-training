# Training tutorials for IMX500

This repository provides tutorials on creating and training different machine learning models for deployment on the Raspberry Pi AI Camera, which uses Sony's IMX500 Intelligent Image Sensor. 
Each tutorial is presented as an interactive [Jupyter notebook](https://docs.jupyter.org/) and contains instructions for dataset setup, model creation, training and quantization. The quantization step is done using the [Model Compression Toolkit (MCT)](https://github.com/sony/model_optimization).

To run the Jupyter notebooks, we recommend using the Google Colab links provided at the beginning of each tutorial and in this README file. If the link does not work, you can download the Jupyter notebook and upload it to Google Colab without any issues. For more advanced usage, or if you prefer to run the tutorials locally, each tutorial includes a Docker image with all necessary dependencies pre-installed.

## Notice

### Security

Please read the Site Policy of GitHub and understand the usage conditions.

## Running tutorials on Colab

### [Training mobilenetV2 classifier](./notebooks/mobilenet-rps/custom_mobilenet.ipynb)
* [Notebook file](notebooks/mobilenet-rps/custom_mobilenet.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training/blob/main/notebooks/mobilenet-rps/custom_mobilenet.ipynb)

### [Training NanoDet object detector](./notebooks/nanodet-ppe/custom_nanodet.ipynb)
* [Notebook file](./notebooks/nanodet-ppe/custom_nanodet.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training/blob/main/notebooks/nanodet-ppe/custom_nanodet.ipynb)

## Running tutorials on Docker
1. Ensure you have Docker installed on your system. You can download and install it from [docker.com](https://www.docker.com/).

2. Clone this repository
```
$ git clone https://github.com/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training.git
```
3. Navigate to repository folder
```
$ cd aitrios-rpi-tutorials-ai-model-training
```
4. Navigate to the desired tutorial folder
```
$ cd notebooks/<tutorial>
```
5. Run the docker container
```
$ make jupyter-local
```
6. Access Jupyter Notebook, fill in the exposed `port`
* Open your browser and navigate to `http://localhost:<port>`.
* Use the token provided in the terminal output to log in.
