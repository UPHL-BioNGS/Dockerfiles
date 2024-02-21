FROM python:3.11-slim as app

ARG SOFTWARENAME_VER="0.1.20240104"
ARG MINIMAP2_VER="2.26"

RUN apt-get update && apt-get install -y --no-install-recommends \
    procps \
    ca-certificates && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

RUN pip install circulocov==${SOFTWARENAME_VER} && circulocov -h

RUN curl -L https://github.com/lh3/minimap2/releases/download/v${MINIMAP2_VER}/minimap2-${MINIMAP2_VER}_x64-linux.tar.bz2 | tar -jxvf -

ENV PATH="/minimap2-${MINIMAP2_VER}_x64-linux/:$PATH" \
    LC_ALL=C

CMD [ "circulocov", "--help" ]

WORKDIR /data
