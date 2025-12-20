FROM debian:bookworm-slim
LABEL name='helm3'

RUN apt-get update && \
    apt-get install -y --no-install-recommends curl gpg apt-transport-https ca-certificates && \
    update-ca-certificates && \
    curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | tee /usr/share/keyrings/helm.gpg > /dev/null && \
    echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | tee /etc/apt/sources.list.d/helm-stable-debian.list && \
    apt-get update && \
    apt-get install helm