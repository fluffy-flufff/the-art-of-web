# syntax=docker/dockerfile:1

FROM python:3.14-slim

ARG CHROME_CHANNEL=stable
ARG GECKODRIVER_VER=0.36.0

ENV PYTHONUNBUFFERED=1 \
    DEBIAN_FRONTEND=noninteractive \
    CHROME_BIN=/usr/bin/chromium \
    GECKO_BIN=/usr/local/bin/firefox

RUN apt-get update && apt-get install -y --no-install-recommends \
    bat \
    binutils \
    build-essential \
    ca-certificates \
    chromium \
    chromium-driver \
    cmake \
    curl \
    dnsutils \
    fd-find \
    file \
    firefox-esr \
    fish \
    git \
    gnupg \
    htop \
    iproute2 \
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

RUN wget -q -O - "https://github.com/mozilla/geckodriver/releases/download/v${GECKODRIVER_VER}/geckodriver-v${GECKODRIVER_VER}-$([ \"$(dpkg --print-architecture)\" = \"arm64\" ] && printf '%s' linux-aarch64 || printf '%s' linux64).tar.gz" | tar xz -C /usr/local/bin

RUN pip install --no-cache-dir \
    flask \
    git+https://github.com/Gallopsled/pwntools#egg=pwntools \
    git+https://github.com/pwncollege/dojjail#egg=dojjail \
    gunicorn \
    ipython \
    psutil \
    pycryptodome \
    requests \
    selenium

RUN mkdir -p /opt/dos && \
    chown -R 0:0 /opt/dos
COPY --chown=0:0 --chmod=755 watchdog /opt/dos/watchdog

RUN ln -s /etc/bash.bashrc /etc/bashrc && \
    chown -R 0:0 /usr/bin /usr/local/bin && \
    find / -xdev -type f -perm -4000 -exec chmod u-s {} \;

ADD --chown=0:0 --chmod=6755 https://github.com/pwncollege/exec-suid/releases/latest/download/exec-suid /usr/bin/exec-suid
ADD --chown=0:0 --chmod=6755 https://github.com/pwncollege/exec-suid/releases/download/v0.1.5/exec-suid /opt/exec-suid

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
