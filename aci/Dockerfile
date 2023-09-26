FROM python:3.9.17

ARG ACI_VER=0.1.20230815

RUN pip install pandas matplotlib pysam

RUN wget -q https://github.com/erinyoung/ACI/archive/refs/tags/${ACI_VER}.tar.gz && \
    tar -xf ${ACI_VER}.tar.gz && \
    rm -rf ${ACI_VER}.tar.gz

ENV PATH=/ACI-${ACI_VER}:$PATH

RUN aci --help

WORKDIR /data

CMD ["aci", "--help"]