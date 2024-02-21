FROM python:3.9.17-slim as app

ARG VER="0.7.2"

LABEL base.image="python:3.9.17-slim"
LABEL dockerfile.version="1"
LABEL software="pango-collapse"
LABEL software.version="${VER}"
LABEL description="collapse lineages up to the first user defined parent lineage"
LABEL website="https://github.com/MDU-PHL/pango-collapse"
LABEL license="https://github.com/MDU-PHL/pango-collapse?tab=GPL-3.0-1-ov-file#readme"
LABEL maintainer="Erin Young"
LABEL maintainer.email="eriny@utah.gov"

RUN apt-get update && apt-get install -y --no-install-recommends \
    procps \
    ca-certificates \
    wget && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

RUN wget -q https://github.com/MDU-PHL/pango-collapse/archive/refs/tags/v${VER}.tar.gz && \
    pip install v${VER}.tar.gz && \
    tar -vxf v${VER}.tar.gz && \
    rm -rf v${VER}.tar.gz && \
    mkdir /pango-collapse && \
    cp /pango-collapse-${VER}/pango_collapse/collapse.txt /pango-collapse && \
    rm -rf /pango-collapse-${VER} && \
    ls /pango-collapse/collapse.txt

ENV PATH="$PATH" \
    LC_ALL=C

# 'CMD' instructions set a default command when the container is run. This is typically 'tool --help.'
CMD pango-collapse --help 

# 'WORKDIR' sets working directory
WORKDIR /data
