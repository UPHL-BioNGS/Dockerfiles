FROM python:3.9.17-slim as app

ARG ACI_VER="1.2.20231229"

# 'LABEL' instructions tag the image with metadata that might be important to the user
LABEL base.image="python:3.9.17-slim"
LABEL dockerfile.version="1"
LABEL software="ACI"
LABEL software.version="${ACI_VER}"
LABEL description="Determines amplicon coverage"
LABEL website="https://github.com/erinyoung/ACI"
LABEL license="https://github.com/erinyoung/ACI/blob/main/LICENSE"
LABEL maintainer="Erin Young"
LABEL maintainer.email="eriny@utah.gov"

RUN apt-get update && apt-get install -y --no-install-recommends \
    procps \
    ca-certificates \
    wget && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

# aci was taken on pip, but not conda
RUN pip install --no-cache amplicon_coverage_inspector==${ACI_VER}

ENV PATH="$PATH" \
    LC_ALL=C

# 'CMD' instructions set a default command when the container is run. This is typically 'tool --help.'
CMD [ "aci", "--help" ]

WORKDIR /data

RUN aci --help
