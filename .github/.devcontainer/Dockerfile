FROM mcr.microsoft.com/devcontainers/base:ubuntu

USER @edpichler
RUN curl --proto '=https' --tlsv1.2 -LsSf https://setup.atuin.sh | sh
USER vscode
RUN curl --proto '=https' --tlsv1.2 -LsSf https://setup.atuin.sh | sh
USER root
RUN curl --proto '=https' --tlsv1.2 -LsSf https://setup.atuin.sh | sh

RUN apt-get update \
    && sudo apt upgrade -y \
    && apt-get -y install --no-install-recommends git git-crypt curl gnupg software-properties-common \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get -y install --no-install-recommends snapd \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*
RUN systemctl enable snapd && \
    systemctl start snapd
RUN snap install hugo

# Clean up unnecessary files
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*