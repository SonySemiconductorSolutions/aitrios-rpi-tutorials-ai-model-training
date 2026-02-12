# Training tutorials for IMX500

This repository provides tutorials on creating and training different machine learning models for deployment on the Raspberry Pi AI Camera, which uses Sony's IMX500 Intelligent Image Sensor. 
Each tutorial is presented as an interactive [Jupyter notebook](https://docs.jupyter.org/) and contains instructions for dataset setup, model creation, training and quantization. The quantization step is done using the [Model Compression Toolkit (MCT)](https://github.com/sony/model_optimization).

To run the Jupyter notebooks, we recommend using the Google Colab links provided at the beginning of each tutorial and in this README file. If the link does not work, you can download the Jupyter notebook and upload it to Google Colab without any issues. For more advanced usage, or if you prefer to run the tutorials locally, each tutorial includes a Docker image with all necessary dependencies pre-installed. More details at the end of this document.

## Notice

### Security

Please read the Site Policy of GitHub and understand the usage conditions.

## Running tutorials on Colab
**Observe**: As of July 2025, Google Colab has been updated and is using Python 3.12. In order to run the following tutorials one **must change the Runtime from Latest to 2025.07**. This will use the previous version of the virtual machine with Python 3.11. For instructions see [blogpost](https://developers.googleblog.com/en/google-colab-adds-more-back-to-school-improvements).

### [Training MobileNetV2 classifier](./notebooks/mobilenet-rps/custom_mobilenet.ipynb)
* [Notebook file](notebooks/mobilenet-rps/custom_mobilenet.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training/blob/main/notebooks/mobilenet-rps/custom_mobilenet.ipynb)

### [Training NanoDet object detector](./notebooks/nanodet-ppe/custom_nanodet.ipynb)
* [Notebook file](./notebooks/nanodet-ppe/custom_nanodet.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training/blob/main/notebooks/nanodet-ppe/custom_nanodet.ipynb)

### [Training Deeplabv3Plus semantic segmentation model](./notebooks/deeplab3-pothole/custom_deeplab.ipynb)
* [Notebook file](./notebooks/deeplab3-pothole/custom_deeplab.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training/blob/main/notebooks/deeplab3-pothole/custom_deeplab.ipynb)

### [Training PersonLab key-points model](./notebooks/personlab-gauge/custom_personlab.ipynb)
* [Notebook file](./notebooks/personlab-gauge/custom_personlab.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training/blob/main/notebooks/personlab-gauge/custom_personlab.ipynb)

## Running tutorials on Docker - VS Code and Docker Setup Instructions for Jupyter Notebooks

To run the Jupyter notebooks, we recommend using VS Code and connect to the Jupyter server running inside a Docker container. This allows you to edit and run the Jupyter Notebooks in VS Code, with all code executing in the containerized environment (resolving dependency issues).

**Follow these steps:**

1. **Install Prerequisites:** Ensure you have **Docker** installed on your system (download from the [Docker website](https://docs.docker.com/engine/install/) if not already). Also install [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview), the VS Code **Python extension** and VS Code **Jupyter extension**. Both extension are provided by Microsoft.

2. **Clone the Repository (if not done already):**

   ```bash
   $ git clone https://github.com/SonySemiconductorSolutions/aitrios-rpi-tutorials-ai-model-training.git
   ```

3. **Navigate to the Repository:**

   ```bash
   $ cd aitrios-rpi-tutorials-ai-model-training
   ```

4. **Go to the Tutorial Directory:** Decide which tutorial you want to run, and navigate to its folder under `notebooks`. For example, for the MobileNet classifier tutorial:

   ```bash
   $ cd notebooks/<tutorial-folder>
   ```

   *(Replace `<tutorial-folder>` with the actual folder name, such as `mobilenet-rps` for the MobilenetV2 classifier, `nanodet-object-detector` for the NanoDet detector, etc.)*

5. **Build and Run the Docker Container with Jupyter:** In the tutorial folder, start the Docker container which will launch a Jupyter Notebook server.

   * **Linux/macOS (or Windows using WSL/Git Bash):** Run the provided Makefile command:

     ```bash
     $ make jupyter-local
     ```

     This will build the Docker image (if not already built) and run a container that starts a Jupyter Notebook server on an available port. The terminal will show logs from the container, including the URL for the running Jupyter server.
   * **Windows (without Make installed):** If the `make` command is not available, you have a couple of options:

     * Install a Make tool or use **WSL** to run the above command.
     * **Or**, run the Docker commands manually. For example, you can build the Docker image and run the container yourself:

       ```bash
       $ docker build -t aitrios_tutorial .
       $ docker run -p 8888:8888 aitrios_tutorial
       ```

       The above commands (run from the tutorial folder) will build the Docker image (tagged as `aitrios_tutorial`) and start a container, mapping port 8888 to your local machine. (Ensure the port `8888` is free or adjust as needed.) The container’s startup will output a URL with a token, similar to the Makefile approach.

6. **Open the Notebook in VS Code:** Launch Visual Studio Code and open the cloned repository folder (or the specific tutorial folder). Then open the Jupyter Notebook file (`.ipynb`) for the tutorial you’re running. The notebook should open in VS Code’s Notebook Editor view.

7. **Connect VS Code to the Docker’s Jupyter Server:** By default, VS Code might try to use a local Python kernel. Instead, point it to the Jupyter server running inside Docker:

   * In the VS Code Notebook Editor, click the **kernel picker** in the top-right corner (it might show a text like “Python 3” or “Select Kernel”).
   * In the kernel selection dropdown, choose **“Existing Jupyter Server…”**. (If you don’t see this option, you can also open the Command Palette with **Ctrl+Shift+P** (Windows/Linux) or **Cmd+Shift+P** (macOS) and run the command **“Jupyter: Specify Jupyter Server for Connections”**.)
   * When prompted for the server URI, type the Jupyter Notebook URL (running Docker container with default settings): `http://localhost:8888`.
   * Press Enter to connect. Press Yes to accept connecting without token. Press Enter to accept Server Display Name (localhost). Press/Select the Jupyter Kernel. VS Code will now attach to the Jupyter server running in the Docker container. You should see an indication in the notebook toolbar or status bar that you’re connected to a remote kernel (for example, it might display “Python 3 (Docker container)” or similar as the selected kernel).

8. **Run the Notebook:** With the kernel connected to the Docker container, you can execute the notebook cells in VS Code as usual (use the Run button (to the left of the cells) or Shift+Enter). All code will run inside the Docker container’s environment. You can verify this by, for example, printing the Python environment or checking for installed packages in the notebook – it should reflect the container’s setup.
Quantized and converted models created inside the Docker container will appear in the shared folder mapped to `$HOST_WORKDIR` (by default `tutorial/`). See the `Makefile` for customization.

9. **(Optional) Stop the Container:** When you are done, stop the Docker container. If you ran it via the Makefile in an attached mode, you can press **Ctrl+C** in that terminal to stop the Jupyter server and then run `docker container ls` and `docker container stop <container-id>` if needed. If you ran it with the `docker run` command manually, you can stop it by pressing Ctrl+C in the terminal that is streaming the logs (since we ran it without `-d` flag), or by using `docker ps` / `docker stop`. Make sure to shut down the container when finished to free resources.
