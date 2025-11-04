# This default is used if --build-arg is not provided
ARG FREYJA_PATHOGEN="SARS-CoV-2"
ARG FREYJA_SOFTWARE_VERSION="2.0.1"

# ---------------------------------------------------------------------
# STAGE 1: Final Application Image ("app")
# ---------------------------------------------------------------------
FROM mambaorg/micromamba:2.3.1-ubuntu22.04 AS app

# Re-declare ARG to make it available in this stage
ARG FREYJA_SOFTWARE_VERSION
# Re-declare ARG to make it available in this stage
ARG FREYJA_PATHOGEN

# build and run as root users since micromamba image has 'mambauser' set as the $USER
USER root
# set workdir to default for building; set to /data at the end
WORKDIR /

LABEL base.image="mambaorg/micromamba:2.3.1-ubuntu22.04"
LABEL dockerfile.version="1"
LABEL software="Freyja"
LABEL software.version=${FREYJA_SOFTWARE_VERSION}
LABEL description="Freyja is a tool to recover relative lineage abundances from mixed SARS-CoV-2 samples from a sequencing dataset (BAM aligned to the Hu-1 reference)"
LABEL website="https://github.com/andersen-lab/Freyja"
LABEL license="https://github.com/andersen-lab/Freyja/blob/main/LICENSE"
LABEL maintainer="Erin Young"
LABEL maintainer.email="eriny@utah.gov"

# set the environment, put new conda env in PATH by default
ENV PATH="/opt/conda/envs/freyja-env/bin:/opt/conda/envs/env/bin:${PATH}" \
    LC_ALL=C.UTF-8

# Create environment, install freyja, clean up, and make /data dir in one layer
RUN micromamba create -n freyja-env -c conda-forge -c bioconda freyja=${FREYJA_SOFTWARE_VERSION} && \
    micromamba clean -a -y -f && \
    mkdir /data

# update barcodes
RUN freyja update --pathogen ${FREYJA_PATHOGEN}

# set working directory to /data
WORKDIR /data

# default command is to pull up help options
CMD [ "freyja", "--help" ]

# ---------------------------------------------------------------------
# STAGE 2: Testing Stage ("test")
# ---------------------------------------------------------------------
FROM app AS test

# Re-declare ARGs to make them available in this stage
ARG FREYJA_SOFTWARE_VERSION
ARG FREYJA_PATHOGEN

# Simple "is it installed" check
RUN freyja --help && freyja --version

# install dependencies for testing; cleanup apt garbage in the same layer
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    ca-certificates \
    procps \
    git && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

# print barcode version and freyja version
RUN freyja demix --pathogen ${FREYJA_PATHOGEN} --version

RUN micromamba list -n freyja-env
