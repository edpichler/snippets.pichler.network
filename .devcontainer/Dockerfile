FROM mcr.microsoft.com/devcontainers/base:ubuntu

USER root
RUN apt-get update && sudo apt upgrade -y
RUN apt-get -y install --no-install-recommends tmux \
    && echo "set -g mouse on" >> /etc/tmux.conf
RUN apt-get -y install --no-install-recommends git git-crypt
RUN apt-get -y install --no-install-recommends gnupg2
RUN apt-get -y install --no-install-recommends curl
RUN apt-get -y install --no-install-recommends neovim dos2unix

# apt hugo
RUN apt-get -y install --no-install-recommends hugo

# apt clean
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Trust in extra SSL certificates
COPY Zscaler_Root_CA.crt /usr/local/share/ca-certificates/
RUN update-ca-certificates

USER vscode
RUN echo "# The zsh files are read in the following order: .zshenv → .zprofile → .zshrc → .zlogin → .zlogout" | tee -a ~/.zshenv ~/.zprofile ~/.zshrc ~/.zlogin ~/.zlogout > /dev/null

RUN curl --proto '=https' --tlsv1.2 -LsSf https://setup.atuin.sh | sh

ARG project_folder=/workspaces/snippets-pichler-network/
RUN echo "cd $project_folder \
    && echo 'Hej Edu!' \
    && echo 'Here are the files and directories:' \
    && find . -maxdepth 1 \
    && echo ''" >> ~/.zshrc

RUN echo "echo 'Syncing atuin:' && atuin sync" >> ~/.zshrc

# extra aliases
RUN echo "# aliases" >> ~/.zaliases
RUN echo "source ~/.zaliases" >> ~/.zshrc

EXPOSE 1313
RUN echo "echo '\nhugo version is: $(hugo version)'" >> ~/.zshrc
