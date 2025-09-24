# syntax=docker/dockerfile:1

FROM python:3.13-slim

ENV PYTHONUNBUFFERED=1 \
    DEBIAN_FRONTEND=noninteractive \
    CHROME_BIN=/usr/bin/chromium \
    GECKO_BIN=/usr/bin/firefox

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    ca-certificates \
    curl \
    wget \
    unzip \
    gnupg \
    lsb-release \
    libssl-dev \
    libffi-dev \
    libpcap-dev \
    libnss3 \
    libxss1 \
    libasound2 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libcups2 \
    libdrm2 \
    libxrandr2 \
    libxcomposite1 \
    libxdamage1 \
    libx11-6 \
    libgtk-3-0 \
    chromium \
    chromium-driver \
    firefox-esr \
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

RUN find / -xdev -type f -perm -4000 -exec chmod u-s {} \;

ADD --chown=0:0 --chmod=6755 http://github.com/pwncollege/exec-suid/releases/latest/download/exec-suid /usr/bin/exec-suid
