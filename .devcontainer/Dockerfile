# Creates development machine inside docker
# so every developer will have the same environment

FROM ubuntu:24.04

ENV NODE_VERSION 22.2.0
ENV PNPM_VERSION 9.1.1

RUN apt-get update \
  && apt-get install -y eza zsh git tig ripgrep bat curl tar micro \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*
ADD --checksum=sha256:2c6eaf8bfd0f886ed8764ffce19c795e909639105e4056b1d9f8f917bad12cf1 https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.gz /usr/local
RUN tar --remove-files -xz -C /usr/local --strip-components=1 \
  --exclude='*.md' --exclude='LICENSE' \
  --exclude='share' --exclude='lib/node_modules/' \
  --exclude='bin/npm' --exclude='bin/npx' --exclude='bin/corepack' \
  -f /usr/local/node-v${NODE_VERSION}-linux-x64.tar.gz
ADD --checksum=sha256:bb187b5c2d937e4389983fb4d79a7dbabbaf320cafd4a93e1c8f4da6dc60fb30 https://github.com/pnpm/pnpm/releases/download/v$PNPM_VERSION/pnpm-linux-x64 /usr/local/bin/
RUN mv /usr/local/bin/pnpm-linux-x64 /usr/local/bin/pnpm
RUN chmod a+x /usr/local/bin/pnpm

RUN userdel -r ubuntu
RUN useradd -m -s /bin/zsh developer
USER developer

RUN pnpm config set store-dir /home/developer/.local/share/pnpm/store \
  && pnpm config set ignore-scripts false
RUN echo 'PS1="%d$ "' > ~/.zshrc
