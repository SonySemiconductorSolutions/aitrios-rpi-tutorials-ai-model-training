TESTDIR := /tmp/tutorial
SRC := custom_mobilenet.ipynb
CHECK_FILE := $(TESTDIR)/models/mobilenet-quant-rps.keras
REPORT_FILE_NAME := executed_notebook.ipynb
REPORT_FILE_PATH := $(TESTDIR)/$(REPORT_FILE_NAME)
PORT := 127.0.0.1
ALLOW_ROOT :=

SHELL := /bin/bash

docker-image:
	docker build -t test-image .

# Use to run notebook manually
jupyter-local: docker-container-remove docker-image
	docker run -it -d --name test-container --shm-size=1g -p 8888:8888 test-image bash
	docker exec -it test-container bash -c "jupyter notebook --ip=$(PORT) $(ALLOW_ROOT) --no-browser --NotebookApp.token=''"

# This is the test to run using github actions
test-github:
	@echo "Running tests..."
	jupyter-nbconvert --to notebook --execute $(SRC) --output $(REPORT_FILE_NAME)
	# If file absent then make will produce error
	[ -f $(CHECK_FILE) ]
	[ -f $(REPORT_FILE_PATH) ]
	@echo Tests OK

docker-container-remove:
	docker container rm -f test-container
	