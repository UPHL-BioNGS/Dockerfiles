FROM mambaorg/micromamba:1.5.1

ARG MEDAKA_VER=1.11.1
ARG PYABPOA_VER=1.2.4

LABEL base.image="mambaorg/micromamba:1.5.1"
LABEL dockerfile.version="1"
LABEL software="medaka"
LABEL software.version="${MEDAKA_VER}"
LABEL description="For polishing assemblies with nanopore reads"
LABEL website="https://github.com/nanoporetech/medaka"
LABEL license="https://github.com/nanoporetech/medaka/blob/master/LICENSE.md"
LABEL maintainer="Erin Young"
LABEL maintainer.email="eriny@utah.gov"

USER root
WORKDIR /

# install dependencies (most are for pyabpoa)
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
    libssl-dev && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

# setting PATH
ENV PATH="${PATH}:/opt/conda/bin/" \
    LC_ALL=C.UTF-8

# no version here because the medaka on conda lags behind the latest release
RUN micromamba install --name base -c conda-forge -c bioconda -c defaults medaka && \
    micromamba clean -a -y && \
    mkdir /data

# this _should_ overwrite the medaka installed via micromamba
RUN /opt/conda/bin/pip install --no-cache medaka==${MEDAKA_VER} && \
    pip install pyabpoa==${PYABPOA_VER} && \
    medaka --version

CMD medaka --help

WORKDIR /data
