FROM ubuntu:jammy

ARG DATASETS_VER="15.12.0"

RUN apt-get update && apt-get install -y --no-install-recommends \ 
    wget \ 
    ca-certificates \ 
    python3 \ 
    unzip && \ 
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/bin

RUN wget -q https://github.com/ncbi/datasets/releases/download/v${DATASETS_VER}/linux-amd64.cli.package.zip && \ 
    unzip linux-amd64.cli.package.zip && \ 
    rm linux-amd64.cli.package.zip && \ 
    chmod +x dataformat datasets

ENV LC_ALL=C

WORKDIR /data

CMD datasets