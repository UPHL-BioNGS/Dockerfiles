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

# Run tests
# This block is now more robust:
# 1. It uses the fixed 'if' statement
# 2. It clones the Freyja repo to get test data matching the software version
RUN if [ "$FREYJA_PATHOGEN" = "SARS-CoV-2" ] ; then \
    # Clone the repo, checking out the specific version tag
    # This is much safer than hardcoding URLs from a different version
    git clone --depth 1 --branch v${FREYJA_SOFTWARE_VERSION} https://github.com/andersen-lab/Freyja.git /tmp/freyja-repo && \
    \
    # Copy the necessary test files into our /data working directory
    cp /tmp/freyja-repo/tests/data/Freyja_WWSC2.bam /data/Freyja_WWSC2.bam && \
    cp /tmp/freyja-repo/tests/data/Freyja_depths.tsv /data/Freyja_depths.tsv && \
    cp /tmp/freyja-repo/tests/data/Freyja_variants.tsv /data/Freyja_variants.tsv && \
    cp /tmp/freyja-repo/tests/data/nCoV-2019.reference.fasta /data/nCoV-2019.reference.fasta && \
    \
    # Run Freyja commands
    freyja variants /data/Freyja_WWSC2.bam --variants /data/test_variants.tsv --depths /data/test_depths.tsv --ref /data/nCoV-2019.reference.fasta && \
    freyja demix /data/test_variants.tsv /data/test_depths.tsv --output /data/test_demix.tsv && \
    \
    # Check validity of outputs
    echo "--- Checking test_variants.tsv ---" && head /data/test_variants.tsv && \
    echo "--- Checking test_depths.tsv ---" && head /data/test_depths.tsv && \
    echo "--- Checking test_demix.tsv ---" && head /data/test_demix.tsv && \
    grep "Omicron" /data/test_demix.tsv && \
    \
    # Clean up the repo
    rm -rf /tmp/freyja-repo; \
    fi

# print barcode version and freyja version
RUN freyja demix --version

RUN micromamba list -n freyja-env
