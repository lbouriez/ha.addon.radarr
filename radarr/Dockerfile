ARG BUILD_FROM=ghcr.io/linuxserver/radarr:amd64-4.0.0-nightly

# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Version
ARG \
    BASHIO_VERSION=0.14.3 \
    TEMPIO_VERSION=2021.09.0

# Environment variables
ENV \
    LANG="C.UTF-8" \
    S6_BEHAVIOUR_IF_STAGE2_FAILS=2 \
    S6_CMD_WAIT_FOR_SERVICES=1

# Copy root filesystem
COPY rootfs /

# Base system
WORKDIR /usr/src
ARG BUILD_ARCH

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
# hadolint ignore=DL3008
RUN \
    set -o pipefail \
    \
    set -x \
    && apt-get update && apt-get install -y --no-install-recommends \
        bash \
        jq \
        tzdata \
        curl \
        ca-certificates \
    \
    && mkdir -p /etc/fix-attrs.d \
    && mkdir -p /etc/services.d \
    \
    && curl -J -L -o /tmp/bashio.tar.gz \
        "https://github.com/hassio-addons/bashio/archive/v${BASHIO_VERSION}.tar.gz" \
    && mkdir /tmp/bashio \
    && tar zxvf \
        /tmp/bashio.tar.gz \
        --strip 1 -C /tmp/bashio \
    \
    && mv /tmp/bashio/lib /usr/lib/bashio \
    && ln -sf /usr/lib/bashio/bashio /usr/bin/bashio \
    \
    && curl -L -f -s -o /usr/bin/tempio \
        "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}" \
    && chmod a+x /usr/bin/tempio \
    \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/src/*