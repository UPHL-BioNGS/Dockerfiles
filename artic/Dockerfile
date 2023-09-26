FROM mambaorg/micromamba:1.4.9

ARG ARTIC_VER=1.2.4
ARG MEDAKA_VER=1.9.1

USER root
WORKDIR /

RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    ca-certificates \
    procps \
    gcc \
    make \
    pkg-config \
    zlib1g-dev \
    libbz2-dev \
    liblzma-dev \
    libcurl4-gnutls-dev \
    libssl-dev \
    python3-dev \
    python3-pip \
    python-is-python3 && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

RUN wget -q https://github.com/artic-network/fieldbioinformatics/archive/refs/tags/v${ARTIC_VER}.tar.gz && \
    tar -xzf v${ARTIC_VER}.tar.gz && \
    micromamba env create -y -f /fieldbioinformatics-${ARTIC_VER}/environment.yml && \
    rm v${ARTIC_VER}.tar.gz && \
    cd fieldbioinformatics-${ARTIC_VER} && \
    python setup.py install && \
    artic -v && \
    mkdir /data

ENV ENV_NAME="artic"
ARG MAMBA_DOCKERFILE_ACTIVATE=1

RUN /opt/conda/envs/artic/bin/pip install setuptools wheel cython medaka==${MEDAKA_VER} && \
    medaka --version && \
    /opt/conda/envs/artic/bin/pip install pyabpoa==1.2.4

ENV PATH="${PATH}:/opt/conda/envs/artic/bin/" \
    LC_ALL=C.UTF-8

CMD artic --help

WORKDIR /data