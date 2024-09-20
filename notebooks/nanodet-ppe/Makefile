PORT := 127.0.0.1
ALLOW_ROOT :=

SHELL := /bin/bash

docker-image:
	docker build -t test-image .

# Use to run notebook manually
jupyter-local: docker-container-remove docker-image
	docker run -it -d --name test-container --shm-size=12g -p 8888:8888 test-image bash
	docker exec -it test-container bash -c "jupyter notebook --ip=$(PORT) $(ALLOW_ROOT) --no-browser --NotebookApp.token=''"

docker-container-remove:
	docker container rm -f test-container
