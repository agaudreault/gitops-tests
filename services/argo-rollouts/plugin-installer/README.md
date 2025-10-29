# Argo Rollouts Step Plugin Installer

This directory contains the Dockerfile and build configuration for the Argo Rollouts Step Plugin installer container.

## Build Instructions

### Prerequisites

- Docker with buildx support
- Make

### Basic Build

To build with default settings (quay.io/argoprojlabs registry and linux/arm64 platform):

```bash
make build
```

### Custom Registry

You can override the default registry by setting the `IMAGE_REGISTRY` environment variable:

```bash
# Example with a custom registry
IMAGE_REGISTRY=my-registry.example.com/my-team make build
```

### Custom Platform

To build for a different platform, use the `TARGET_ARCH` variable:

```bash
# Example for AMD64
TARGET_ARCH=linux/amd64 make build

# Example for ARM64
TARGET_ARCH=linux/arm64 make build

# Example for multiple platforms
TARGET_ARCH="linux/amd64,linux/arm64" make build
```

### Custom Registry and Platform Combined

You can combine both custom registry and platform:

```bash
IMAGE_REGISTRY=my-registry.example.com/my-team TARGET_ARCH=linux/amd64 make build
```

## Image Details

The built image will be tagged as:

- Image Name: `argo-rollouts-step-plugin-installer`
- Tag: `latest`
- Full Image Path: `${IMAGE_REGISTRY}/argo-rollouts-step-plugin-installer:latest`

Where `IMAGE_REGISTRY` defaults to `quay.io/argoprojlabs/rollouts-plugins` if not specified.

## Integration with Argo Rollouts

The image reference in your Argo Rollouts configuration (kustomization.yaml) needs to match the built image path. If you use a custom registry, make sure to update your kustomization.yaml accordingly.
