TESTDIR := /home/newuser/tutorial
SRC := custom_nanodet.ipynb
CHECK_FILE := $(TESTDIR)/nanodet-quant-ppe.keras
REPORT_FILE_NAME := executed_notebook.ipynb
REPORT_FILE_PATH := $(TESTDIR)/$(REPORT_FILE_NAME)
IP_ADDR := 127.0.0.1
PORT := 8888
ALLOW_ROOT :=

# Host folder that mirrors TESTDIR inside the container
HOST_WORKDIR ?= $(CURDIR)/tutorial

SHELL := /bin/bash

.PHONY: check-gpu-deps

docker-image:
	# Build with your host UID/GID so mounted files keep correct ownership
	docker build \
		--build-arg NB_UID=$$(id -u) \
		--build-arg NB_GID=$$(id -g) \
		-t test-image .

prepare-host-dir:
	mkdir -p "$(HOST_WORKDIR)"

# Use to run notebook manually (with persistence)
jupyter-local: check-gpu-deps docker-container-remove docker-image prepare-host-dir
	docker run -it -d --name test-container \
		--gpus all --shm-size=1g \
		-p $(IP_ADDR):$(PORT):$(PORT) \
		-v "$(HOST_WORKDIR):$(TESTDIR)" \
		test-image bash
	# Start Jupyter from the work dir so new files land in the mounted folder
	docker exec -it test-container bash -lc "cd '$(TESTDIR)' && jupyter notebook --ip=0.0.0.0 $(ALLOW_ROOT) --no-browser --NotebookApp.token=''"

# This is the test to run using github actions
test-github:
	@echo "Running tests..."
	jupyter-nbconvert --to notebook --execute $(SRC) --output $(REPORT_FILE_NAME)
	# If file absent then make will produce error
	[ -f $(CHECK_FILE) ]
	[ -f $(REPORT_FILE_PATH) ]
	@echo Tests OK

check-gpu-deps:
	@ok=1; \
	if ! command -v nvidia-smi >/dev/null 2>&1; then \
	  echo "ERROR: 'nvidia-smi' not found. A working NVIDIA driver + GPU on the host is required."; \
	  ok=0; \
	else \
	  if ! nvidia-smi >/dev/null 2>&1; then \
	    echo "ERROR: 'nvidia-smi' is present but cannot communicate with the NVIDIA driver."; \
	    ok=0; \
	  fi; \
	fi; \
	if ! command -v nvidia-ctk >/dev/null 2>&1; then \
	  echo "ERROR: NVIDIA Container Toolkit ('nvidia-ctk') not found."; \
	  echo "       It is required for 'docker --gpus ...' to work. Please install and configure it."; \
	  ok=0; \
	fi; \
	if [ $$ok -eq 1 ]; then \
	  echo "OK: GPU driver (nvidia-smi) and NVIDIA Container Toolkit are available."; \
	else \
	  echo "FAIL: One or more GPU prerequisites are missing (see messages above)."; \
	  exit 1; \
	fi


docker-container-remove:
	docker container rm -f test-container 2>/dev/null || true
