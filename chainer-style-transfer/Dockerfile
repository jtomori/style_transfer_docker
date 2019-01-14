FROM chainer/chainer:v5.1.0-python3

# Install packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    python3-pil && \
    apt-get autoclean && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Clone repo, install requirements
RUN git clone --single-branch --branch resize-conv https://github.com/jtomori/chainer-fast-neuralstyle.git

WORKDIR /app/work