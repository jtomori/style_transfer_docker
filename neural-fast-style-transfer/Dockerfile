FROM tensorflow/tensorflow:1.12.0-gpu-py3

# Install packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    ffmpeg && \
    apt-get autoclean && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Download pre-trained VGG network
RUN curl http://www.vlfeat.org/matconvnet/models/imagenet-vgg-verydeep-19.mat --output imagenet-vgg-verydeep-19.mat

# Clone repos, install requirements
RUN git clone https://github.com/anishathalye/neural-style.git && \
    git clone https://github.com/jtomori/fast-style-transfer.git && \
    pip install moviepy

# Let fast-style-transfer work outside of its repo directory
ENV PYTHONPATH /app/fast-style-transfer/src

WORKDIR /app/work