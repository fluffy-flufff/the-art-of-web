# syntax=docker/dockerfile:1

FROM python:3.13-slim

ARG CHROME_CHANNEL=stable
ENV PYTHONUNBUFFERED=1 \
    DEBIAN_FRONTEND=noninteractive \
    CHROME_BIN=/usr/bin/chromium \
    GECKO_BIN=/usr/bin/firefox

RUN apt-get update && apt-get install -y --no-install-recommends \
    bat \
    binutils \
    build-essential \
    ca-certificates \
    chromium \
    chromium-driver \
    curl \
    dnsutils \
    fd-find \
    file \
    fish \
    firefox-esr \
    git \
    gnupg \
    htop \
    iputils-ping \
    jq \
    less \
    libasound2 \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libcups2 \
    libdrm2 \
    libffi-dev \
    libgtk-3-0 \
    libnss3 \
    libpcap-dev \
    libssl-dev \
    libx11-6 \
    libxcomposite1 \
    libxdamage1 \
    libxrandr2 \
    libxss1 \
    lsb-release \
    lsof \
    lz4 \
    moreutils \
    ncurses-bin \
    ncurses-term \
    neovim \
    netcat-openbsd \
    nmap \
    openssl \
    p7zip-full \
    parallel \
    psmisc \
    pv \
    rename \
    ripgrep \
    rsync \
    socat \
    sudo \
    tcpdump \
    tmux \
    traceroute \
    tree \
    unar \
    unzip \
    vim \
    wget \
    whois \
    zip \
    zsh \
    zstd \
    && rm -rf /var/lib/apt/lists/*

RUN <<EOF
    wget -q -O - https://github.com/mozilla/geckodriver/releases/download/v0.36.0/geckodriver-v0.36.0-linux64.tar.gz | tar xvz -C /usr/bin
EOF

RUN pip install --no-cache-dir \
    flask \
    git+https://github.com/pwncollege/dojjail#egg=dojjail \
    git+https://github.com/Gallopsled/pwntools#egg=pwntools \
    gunicorn \
    ipython \
    jupyter \
    psutil \
    pycryptodome \
    requests \
    selenium

RUN ln -s /etc/bash.bashrc /etc/bashrc && \
    find / -xdev -type f -perm -4000 -exec chmod u-s {} \;

ADD --chown=0:0 --chmod=6755 http://github.com/pwncollege/exec-suid/releases/latest/download/exec-suid /usr/bin/exec-suid

# chrome sandbox wants root owner and 4755
# still broken anyway so need to use with --no-sandbox
RUN set -eux && \
    arch="$(dpkg --print-architecture)" && \
    [ "$arch" != "amd64" ] || { \
    apt-get update && \
    wget -O /tmp/google-chrome.deb "https://dl.google.com/linux/direct/google-chrome-${CHROME_CHANNEL}_current_${arch}.deb" && \
    (apt-get install -y /tmp/google-chrome.deb || apt-get -f install -y) && \
    rm -f /tmp/google-chrome.deb && \
    rm -rf /var/lib/apt/lists/*; \
    }
