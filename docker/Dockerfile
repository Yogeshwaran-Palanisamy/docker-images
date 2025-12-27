# Stage 1: Builder - Install all dependencies and create virtual environment
FROM debian:bookworm-slim AS builder
LABEL name='helm3'

# Install build tools and dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    gpg \
    apt-transport-https \
    ca-certificates \
    pkg-config \
    python3-venv \
    python3-pip \
    build-essential \
    libssl-dev \
    libffi-dev \
    python3-dev \
    libgtk-3-dev \
    libwebkit2gtk-4.0-dev \
    && update-ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Install Helm
RUN curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | tee /usr/share/keyrings/helm.gpg > /dev/null && \
    echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | tee /etc/apt/sources.list.d/helm-stable-debian.list && \
    apt-get update && \
    apt-get install -y helm \
    && rm -rf /var/lib/apt/lists/*

# Create the virtual environment
RUN python3 -m venv /opt/venv

# Activate venv and install Python packages
ENV PATH="/opt/venv/bin:$PATH"
RUN pip install --no-cache-dir yq pyyaml requests

# Stage 2: Runtime - Minimal image with only runtime dependencies
FROM debian:bookworm-slim
LABEL name='helm3'

# Install only runtime dependencies (no build tools)
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    ca-certificates \
    libgtk-3-0 \
    libwebkit2gtk-4.0-37 \
    && rm -rf /var/lib/apt/lists/*

# Copy Helm from builder
COPY --from=builder /usr/bin/helm /usr/local/bin/helm

# Copy virtual environment from builder
COPY --from=builder /opt/venv /opt/venv

# Set PATH to use virtual environment
ENV PATH="/opt/venv/bin:$PATH"
