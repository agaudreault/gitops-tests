# Build stage
FROM golang:1.23-alpine AS builder

# Set working directory
WORKDIR /workspace

# Copy Argo Rollouts repository
RUN apk add --no-cache git && \
    git clone --depth 1 https://github.com/argoproj/argo-rollouts.git .

# Build arguments for target platform
ARG TARGETOS
ARG TARGETARCH

# Build the plugin binary
RUN CGO_ENABLED=0 GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -gcflags="all=-N -l" -o /plugin-bin/e2e-step-plugin test/cmd/step-plugin-e2e/main.go

# Final stage
FROM alpine:3.19

# Create a non-root user and group for running the container
RUN addgroup -g 1000 plugin && \
    adduser -D -u 1000 -G plugin -h /home/plugin -s /sbin/nologin plugin && \
    mkdir -p /plugin-bin && chown plugin:plugin /plugin-bin && chmod 0755 /plugin-bin

# Copy the plugin binary from builder and set ownership to the non-root user
# Use --chown so the binary is writable/executable by the non-root user
COPY --from=builder --chown=plugin:plugin /plugin-bin/e2e-step-plugin /plugin-bin/e2e-step-plugin

# Ensure the binary is executable
RUN chmod 0755 /plugin-bin/e2e-step-plugin

# Run as non-root user by default
USER 1000

# Default command does nothing by design; the container image is an "installer" that
# can be run to extract the binary into a mounted volume. Keep behaviour minimal so
# callers can mount volumes at /plugin-bin or invoke the binary directly.
