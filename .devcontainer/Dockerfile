FROM mcr.microsoft.com/devcontainers/base:ubuntu

USER root
RUN apt-get update && sudo apt upgrade -y
RUN apt-get -y install --no-install-recommends tmux \
    && echo "set -g mouse on" >> /etc/tmux.conf
RUN apt-get -y install --no-install-recommends git git-crypt
RUN apt-get -y install --no-install-recommends gnupg2
RUN apt-get -y install --no-install-recommends curl
RUN apt-get -y install --no-install-recommends neovim dos2unix
RUN apt-get -y install --no-install-recommends hugo
RUN apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*

ARG project_folder=/workspaces/snippets-pichler-network/

# Copy SSL certificates to the container
COPY Zscaler_Root_CA.crt /usr/local/share/ca-certificates/
# Update SSL certificates in the container
RUN update-ca-certificates

USER vscode
# RUN echo insecure >> ~/.curlrc
RUN curl --proto '=https' --tlsv1.2 -LsSf https://setup.atuin.sh | sh

RUN echo "cd $project_folder \
    && echo 'Hej Edu!' \
    && echo 'Here are the files and directories:' \
    && find . -maxdepth 1 \
    && echo '' \
    && echo 'Syncing atuin:' \
    && atuin sync" >> ~/.bashrc

RUN echo "echo 'Syncing atuin:' && atuin sync" >> ~/.bashrc

RUN git config --global --add safe.directory $project_folder

EXPOSE 1313
