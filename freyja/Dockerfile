FROM mambaorg/micromamba:1.4

ARG FREYJA_SOFTWARE_VERSION="1.4.7"

USER root
WORKDIR /

RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    ca-certificates \
    procps && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

RUN micromamba create -n freyja-env -c conda-forge -c bioconda -c defaults freyja=${FREYJA_SOFTWARE_VERSION} && \
    micromamba clean -a -y

ENV PATH="/opt/conda/envs/freyja-env/bin:/opt/conda/envs/env/bin:${PATH}" \
    LC_ALL=C.UTF-8

RUN freyja update

RUN wget https://raw.githubusercontent.com/andersen-lab/Freyja/main/freyja/data/usher_barcodes.csv && \
    mv -f usher_barcodes.csv /opt/conda/envs/freyja-env/lib/python3.10/site-packages/freyja/data/usher_barcodes.csv

WORKDIR /data

CMD freyja --help