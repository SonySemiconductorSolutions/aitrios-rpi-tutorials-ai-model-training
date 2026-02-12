TESTDIR := /home/newuser/tutorial
SRC := custom_personlab.ipynb
CHECK_FILE := aitrios-rpi-training-samples/samples/model/posenet_digitalgauge/posenet_digitalgauge_quantized.keras
REPORT_FILE_NAME := executed_notebook.ipynb
REPORT_FILE_PATH := $(REPORT_FILE_NAME)
IP_ADDR := 127.0.0.1
PORT := 8888
ALLOW_ROOT :=

# Host folder that mirrors TESTDIR inside the container
HOST_WORKDIR ?= $(CURDIR)/tutorial

SHELL := /bin/bash

docker-image:
	# Build with your host UID/GID so mounted files keep correct ownership
	docker build \
		--build-arg NB_UID=$$(id -u) \
		--build-arg NB_GID=$$(id -g) \
		-t test-image .

prepare-host-dir:
	mkdir -p "$(HOST_WORKDIR)"

# Use to run notebook manually (with persistence)
jupyter-local: docker-container-remove docker-image prepare-host-dir
	docker run -it -d --name test-container \
		--gpus all --shm-size=2g \
		-p $(IP_ADDR):$(PORT):$(PORT) \
		-v $(HOST_WORKDIR):$(TESTDIR) \
		test-image bash
	# Start Jupyter from the work dir so new files land in the mounted folder
	docker exec -it test-container bash -lc "jupyter notebook --ip=0.0.0.0 $(ALLOW_ROOT) --no-browser --NotebookApp.token=''"

# This is the test to run using github actions
test-github:
	@echo "Running tests..."
	jupyter-nbconvert --to notebook --execute $(SRC) --output $(REPORT_FILE_NAME)
	# If file absent then make will produce error
	[ -f $(CHECK_FILE) ]
	[ -f $(REPORT_FILE_PATH) ]
	@echo Tests OK

docker-container-remove:
	docker container rm -f test-container 2>/dev/null || true
