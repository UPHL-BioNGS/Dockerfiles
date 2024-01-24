FROM staphb/ncbi-datasets:16.2.0 as fasta

    WORKDIR /ref

    COPY accessions.txt .
    COPY rename.sh .

    RUN grep -v ^# accessions.txt > ids.txt && head ids.txt

    RUN datasets download genome accession --inputfile ids.txt

    RUN unzip *zip

    RUN mkdir prep && bash rename.sh

FROM ubuntu:jammy as app

WORKDIR /ref

COPY --from=fasta /ref/prep /ref

# install dependencies; cleanup apt garbage
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    ca-certificates \
    procps && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

WORKDIR /data

RUN ls /ref

CMD ls /ref