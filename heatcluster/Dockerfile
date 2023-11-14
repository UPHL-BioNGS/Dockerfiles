FROM ubuntu:jammy

ARG HEATCLUSTER_VER="0.4.12"

LABEL base.image="ubuntu:jammy"
LABEL dockerfile.version="1"
LABEL software="HeatCluster"
LABEL software.version="${HEATCLUSTER_VER}"
LABEL description="Visualize SNP matrics from SNP-dists output"
LABEL website="https://github.com/DrB-S/heatcluster"
LABEL license="https://github.com/DrB-S/heatcluster/blob/main/LICENSE"
LABEL maintainer="Erin Young"
LABEL maintainer.email="eriny@utah.gov"

RUN apt-get update && apt-get install -y --no-install-recommends \
  ca-certificates \
  wget \
  procps \
  python3 \
  python3-pip && \
  apt-get autoclean && rm -rf /var/lib/apt/lists/*

# Install Python packages
RUN pip3 install --no-cache argparse pandas numpy pathlib seaborn matplotlib scipy 

RUN wget -q https://github.com/DrB-S/heatcluster/archive/refs/tags/v${HEATCLUSTER_VER}.tar.gz && \
  tar -vxf v${HEATCLUSTER_VER}.tar.gz && \
  rm v${HEATCLUSTER_VER}.tar.gz && \
  mkdir /data

ENV PATH=/heatcluster-${HEATCLUSTER_VER}:$PATH

RUN heatcluster.py --help

CMD heatcluster.py --help

WORKDIR /data
