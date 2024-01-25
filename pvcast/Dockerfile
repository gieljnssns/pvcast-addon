ARG BUILD_FROM


# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Add pvcast source code
ADD /pvcast /pvcast

# Copy Python requirements file
COPY requirements_dev.txt /tmp/requirements_dev.txt

# Copy root filesystem
COPY rootfs /

# Copy config files
COPY config/ /etc/

# Update package lists and install essential packages
RUN apt update && \
    apt-get install -y --no-install-recommends \
    python3-pip \
    software-properties-common && \
    \
    add-apt-repository ppa:deadsnakes/ppa -y && \
    apt update && \
    apt dist-upgrade --autoremove -y && \
    apt install -y --no-install-recommends \
    python3.12 \
    python3.12-dev \
    python3.12-distutils

# Install Python packages
RUN curl -sS https://bootstrap.pypa.io/get-pip.py | python3.12 && \
    python3.12 -m pip install --no-cache-dir --upgrade pip && \
    python3.12 -m pip uninstall --no-cache-dir six -y && \
    python3.12 -m pip install --no-cache-dir six && \
    python3.12 -m pip install --no-cache-dir -r /tmp/requirements_dev.txt && \
    python3.12 -m pip install -e /pvcast && \
    rm -f /tmp/requirements_dev.txt

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Stefan de Lange" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Stefan de Lange" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}