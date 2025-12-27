# Stage 1: Builder - Install all dependencies and create virtual environment
FROM debian:bookworm-slim AS builder
LABEL name='helm3'

# Install build tools and dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    gpg \
    apt-transport-https \
    ca-certificates \
    pkg-config 

# Install Helm
RUN curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | tee /usr/share/keyrings/helm.gpg > /dev/null && \
    echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | tee /etc/apt/sources.list.d/helm-stable-debian.list && \
    apt-get update && \
    apt-get install -y helm \
    && rm -rf /var/lib/apt/lists/*

# Stage 2: Runtime - Minimal image with only runtime dependencies
FROM python:3.11.14-alpine3.23
LABEL name='helm3'

# Copy Helm from builder
COPY --from=builder /usr/bin/helm /usr/local/bin/helm

RUN pip install --no-cache-dir yq pyyaml requests
