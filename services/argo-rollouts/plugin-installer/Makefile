# Variables
PLUGIN_IMAGE_NAME := argo-rollouts-step-plugin-installer
PLUGIN_IMAGE_TAG := latest
TARGET_ARCH ?= linux/arm64
IMAGE_REGISTRY ?= quay.io/argoprojlabs/rollouts-plugins
IMAGE := $(IMAGE_REGISTRY)/$(PLUGIN_IMAGE_NAME):$(PLUGIN_IMAGE_TAG)

.PHONY: build
build:
	# Build for the target architecture and push to the configured registry
	docker build --platform $(TARGET_ARCH) \
		-t $(IMAGE) \
		.

.PHONY: push
push:
	# Build for the target architecture and push to the configured registry
	docker push $(IMAGE)
