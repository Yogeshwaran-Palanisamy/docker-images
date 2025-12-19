FROM node:18-slim
WORKDIR /app
LABEL name='helm3'

RUN apt-get update && \
    apt-get install -y --no-install-recommends curl ca-certificates tar gzip && \
    rm -rf /var/lib/apt/lists/* && \
    curl -fsSL -o /tmp/helm.tar.gz https://get.helm.sh/helm-v3.12.0-linux-amd64.tar.gz && \
    tar -C /tmp -xzf /tmp/helm.tar.gz && \
    mv /tmp/linux-amd64/helm /usr/local/bin/helm && \
    chmod +x /usr/local/bin/helm && \
    rm -rf /tmp/helm.tar.gz /tmp/linux-amd64

CMD ["helm"]