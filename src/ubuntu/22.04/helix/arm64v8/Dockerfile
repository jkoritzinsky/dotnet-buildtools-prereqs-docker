FROM ubuntu.azurecr.io/ubuntu:22.04

# Install Helix Dependencies
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -qq -y \
        autoconf \
        automake \
        build-essential \
        cmake \
        clang \
        curl \
        gcc \
        gdb \
        git \
        gss-ntlmssp \
        iputils-ping \
        libffi-dev \
        libgdiplus \
        libicu-dev \
        libnuma-dev \
        liblttng-ust-dev \
        libssl-dev \
        libtool \
        libunwind8 \
        libunwind-dev \
        lldb-12 \
        llvm \
        locales \
        locales-all \
        python3-dev \
        python3-pip \
        software-properties-common \
        sudo \
        tzdata \
        unzip \
        curl \
    && rm -rf /var/lib/apt/lists/* \
    \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

ENV LANG=en_US.utf8

RUN ln -sf /usr/bin/python3 /usr/bin/python && \
    python -m pip install --upgrade pip==22.0.4 && \
    python -m pip install virtualenv==20.14.0 && \
    pip download --no-deps helix-scripts --index-url https://dnceng.pkgs.visualstudio.com/public/_packaging/helix-client-prod/pypi/simple && \
    export CRYPTOGRAPHY_DONT_BUILD_RUST=1 && \
    pip install ./helix_scripts-*-py3-none-any.whl

# Add MsQuic
RUN curl -LO https://packages.microsoft.com/keys/microsoft.asc && \
    echo 2cfd20a306b2fa5e25522d78f2ef50a1f429d35fd30bd983e2ebffc2b80944fa microsoft.asc| sha256sum --check - && \
    apt-key add microsoft.asc && \
    rm microsoft.asc && \
    apt-add-repository https://packages.microsoft.com/ubuntu/22.04/prod && \
    apt-get update && \
    apt-get install -y libmsquic && \
    rm -rf /var/lib/apt/lists/*

# (we use two users here to ensure volume mounting works with two possible UIDs of the host UID)
RUN adduser --disabled-password --gecos '' --uid 1001 --shell /bin/bash --ingroup adm helixbot && \
    adduser --disabled-password --gecos '' --uid 1000 --shell /bin/bash --ingroup adm helixbot2 && \
    chmod -R +x /root && \
    echo 'helixbot ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers && \
    echo 'helixbot2 ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER helixbot

RUN python -m virtualenv /home/helixbot/.vsts-env
