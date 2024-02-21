FROM python:3.11-slim as app

ARG SOFTWARENAME_VER="0.1.20240104"

RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    ca-certificates \
    bzip2 \
    procps && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

RUN pip install circulocov==${SOFTWARENAME_VER}

RUN curl -L https://github.com/lh3/minimap2/releases/download/v2.26/minimap2-2.26_x64-linux.tar.bz2 | tar -jxvf - --no-same-owner  && \
    ./minimap2-2.26_x64-linux/minimap2 -h

ENV PATH="/minimap2-2.26_x64-linux/:$PATH" \
    LC_ALL=C

RUN minimap2 -h && circulocov -h

CMD [ "circulocov", "--help" ]

WORKDIR /data
