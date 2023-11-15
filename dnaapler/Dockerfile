FROM mambaorg/micromamba:1.4.1 as app

USER root

WORKDIR /

ARG DNAAPLER_VER="0.4.0"

# metadata labels
LABEL base.image="mambaorg/micromamba:1.4.1"
LABEL dockerfile.version="1"
LABEL software="dnaapler"
LABEL software.version="${DNAAPLER_VER}"
LABEL description="Rotates chromosomes and more"
LABEL website="https://github.com/gbouras13/dnaapler"
LABEL license="MIT"
LABEL license.url="https://github.com/gbouras13/dnaapler/blob/main/LICENSE"
LABEL maintainer="Erin Young"
LABEL maintainer.email="eriny@utah.gov"

# install dependencies; cleanup apt garbage
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    ca-certificates \
    procps && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

# create the conda environment, install mykrobe via bioconda package; cleanup conda garbage
RUN micromamba create -n dnaapler -y -c bioconda -c defaults -c conda-forge dnaapler=${DNAAPLER_VER} && \
    micromamba clean -a -y

# set the PATH and LC_ALL for singularity compatibility
ENV PATH="/opt/conda/envs/dnaapler/bin/:${PATH}" \
    LC_ALL=C.UTF-8

# so that mamba/conda env is active when running below commands
ENV ENV_NAME="dnaapler"
ARG MAMBA_DOCKERFILE_ACTIVATE=1

# set final working directory as /data
WORKDIR /data

# default command is to print help options
CMD [ "dnaapler", "--help" ]
