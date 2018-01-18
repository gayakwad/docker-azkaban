GROUP=azkaban
IMAGE_NAME=executor-server
VCS_REF=`git rev-parse --short HEAD`
IMAGE_VERSION=1
DOCKER_REGISTRY=gayakwad
BUILD_NUMBER=1
QNAME=$(DOCKER_REGISTRY)/$(GROUP)/$(IMAGE_NAME)

GIT_TAG=$(QNAME):$(VCS_REF)
BUILD_TAG=$(QNAME):$(BUILD_NUMBER)
LATEST_TAG=$(QNAME):latest

.PHONY: help
.DEFAULT_GOAL := help

build: ## Build the Docker Image
	docker build \
		--build-arg VCS_REF=$(VCS_REF) \
		--build-arg BUILD_NUMBER=$(BUILD_NUMBER) \
		-t $(GIT_TAG) .

lint: ## Lint the Docker Image
	docker run -i --rm -v "`pwd`/Dockerfile:/Dockerfile:ro" redcoolbeans/dockerlint

tag: ## Tag the Docker Image
	echo $(GIT_TAG)
	docker tag $(GIT_TAG) $(BUILD_TAG)
	docker tag $(GIT_TAG) $(LATEST_TAG)

push: ## Push the Docker Image to Registry
	docker push $(GIT_TAG)
	docker push $(BUILD_TAG)
	docker push $(LATEST_TAG)

run: build
	docker run $(GIT_TAG)

help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'
