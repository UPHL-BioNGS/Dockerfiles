FROM python:3.9.17-slim as app

# List all software versions are ARGs near the top of the dockerfile
# 'ARG' sets environment variables during the build stage
# ARG variables are ONLY available during image build, they do not persist in the final image
ARG PHYTREEVIZ_VER="0.1.0"

# 'LABEL' instructions tag the image with metadata that might be important to the user
LABEL base.image="python:3.9.17-slim"
LABEL dockerfile.version="1"
LABEL software="phyTreeViz"
LABEL software.version="${PHYTREEVIZ_VER}"
LABEL description="Visualizing phylogenetic trees"
LABEL website="https://github.com/moshi4/phyTreeViz/"
LABEL license="https://github.com/moshi4/phyTreeViz/blob/main/LICENSE"
LABEL maintainer="Erin Young"
LABEL maintainer.email="eriny@utah.gov"

# 'RUN' executes code during the build
# Install dependencies via apt-get or yum if using a centos or fedora base
RUN apt-get update && apt-get install -y --no-install-recommends \
    procps \
    ca-certificates && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

# Install and/or setup more things. Make /data for use as a working dir
# For readability, limit one install per 'RUN' statement.
RUN pip install --no-cache phytreeviz==${PHYTREEVIZ_VER}

ENV PATH="$PATH" \
    LC_ALL=C

# 'CMD' instructions set a default command when the container is run. This is typically 'tool --help.'
CMD phytreeviz --help 

# 'WORKDIR' sets working directory
WORKDIR /data
