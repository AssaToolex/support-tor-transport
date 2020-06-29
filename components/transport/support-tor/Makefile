#
#
#

ifndef ALPINE_VERSION
	ALPINE_VERSION := latest
endif
ifndef DOCKER_ORGANIZATION
	DOCKER_ORGANIZATION := talex
endif
ifndef DOCKER_IMAGE
	DOCKER_IMAGE := support-tor-transport
endif
ifndef DOCKER_IMAGE_VERSION
	DOCKER_IMAGE_VERSION := latest
endif
DOCKER_IMAGE_FILENAME ?= $(DOCKER_ORGANIZATION)_$(DOCKER_IMAGE)_$(DOCKER_IMAGE_VERSION).tar

all: docker-build docker-test

docker-build:
	sudo docker build -t $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE):$(DOCKER_IMAGE_VERSION) -f Dockerfile-support-tor-transport --build-arg ALPINE_VERSION=$(ALPINE_VERSION) .

docker-test:
	sudo docker image inspect $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE):$(DOCKER_IMAGE_VERSION)
	sudo docker run --rm $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE):$(DOCKER_IMAGE_VERSION) --version

docker-save:
	sudo docker image inspect $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE):$(DOCKER_IMAGE_VERSION) > /dev/null 2>&1
	sudo docker save -o $(DOCKER_IMAGE_FILENAME) $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE):$(DOCKER_IMAGE_VERSION)

check-dockerhub-env:
ifndef DOCKERHUB_USERNAME
	$(error DOCKERHUB_USERNAME is undefined)
endif
ifndef DOCKERHUB_PASSWORD
	$(error DOCKERHUB_PASSWORD is undefined)
endif

dockerhub-push: check-dockerhub-env
	@echo "${DOCKERHUB_PASSWORD}" | sudo docker login -u "${DOCKERHUB_USERNAME}" --password-stdin
	sudo docker push $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE):$(DOCKER_IMAGE_VERSION)
ifdef CIRCLE_TAG
	sudo docker tag $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE):$(DOCKER_IMAGE_VERSION) $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE):${CIRCLE_TAG}
	sudo docker push $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE):${CIRCLE_TAG}
endif
	echo "clear credentials..."
	sudo docker logout

docker-load:
ifneq ($(wildcard $(DOCKER_IMAGE_FILENAME)),)
	sudo docker load -i $(DOCKER_IMAGE_FILENAME)
endif

.PHONY: all check-dockerhub-env docker-build docker-test docker-save dockerhub-push
# vim:ft=make
