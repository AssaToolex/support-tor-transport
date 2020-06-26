ALPINE_VERSION := latest
DOCKER_ORGANIZATION := talex
DOCKER_IMAGE := support-tor-transport
DOCKER_IMAGE_VERSION := latest
DOCKER_IMAGE_FILENAME ?= $(DOCKER_ORGANIZATION)_$(DOCKER_IMAGE).tar

all: docker-build docker-test

docker-build:
	sudo docker build -t $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE) -f Dockerfile-support-tor-transport  --build-arg ALPINE_VERSION=$(ALPINE_VERSION) .

docker-test:
	sudo docker image inspect $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE)
	sudo docker run --rm $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE) --version

docker-save:
	sudo docker image inspect $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE) > /dev/null 2>&1
	sudo docker save -o $(DOCKER_IMAGE_FILENAME) $(DOCKER_ORGANIZATION)/$(DOCKER_IMAGE)

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