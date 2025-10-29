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

# Copy the plugin binary from builder
COPY --from=builder /plugin-bin/e2e-step-plugin /plugin-bin/e2e-step-plugin
