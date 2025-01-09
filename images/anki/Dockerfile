# syntax=docker/dockerfile:1.4
ARG BASE_APP_IMAGE

# hadolint ignore=DL3006
FROM ${BASE_APP_IMAGE}

ARG DEBIAN_FRONTEND=noninteractive
ARG REQUIRED_PACKAGES=" \
    libxcb-xinerama0 \
    libxcb-cursor0 \
    libnss3 \
    zstd \
    ubuntu-restricted-extras \
    wget \
    mpv \
    lame \
    "

RUN <<_INSTALL_PACKAGES
apt-get update
apt-get install -y --no-install-recommends software-properties-common gpg-agent
apt-get install -y --no-install-recommends $REQUIRED_PACKAGES
apt-get remove -y software-properties-common gpg-agent
apt-get autoremove -y
rm -rf /var/lib/apt/lists/*
wget https://github.com/ankitects/anki/releases/download/24.11/anki-24.11-linux-qt6.tar.zst
zstd -d anki-24.11-linux-qt6.tar.zst --stdout | tar -xf -
cd anki-24.11-linux-qt6  && ./install.sh &&  mv anki /usr/bin && cd ..
rm -rf anki-24.11-linux-qt6.tar.zst "anki-24.11-linux-qt6"
apt-get clean
_INSTALL_PACKAGES

COPY --chmod=777 scripts/startup.sh /opt/gow/startup-app.sh

ENV ANKI_WAYLAND=1
ARG IMAGE_SOURCE
LABEL org.opencontainers.image.source=$IMAGE_SOURCE
