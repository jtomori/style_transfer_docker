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

# Run as a non-root user
# TODO - run as host user? (container user uid and gid matching host user)
#RUN groupadd --gid 1001 app && \
#    useradd --uid 1001 --gid 1001 --home-dir /app --create-home --shell /bin/bash --no-log-init app

#USER 1001

WORKDIR /app

# Download pre-trained VGG network
RUN curl http://www.vlfeat.org/matconvnet/models/imagenet-vgg-verydeep-19.mat --output imagenet-vgg-verydeep-19.mat

# Clone repos, install requirements
RUN git clone https://github.com/anishathalye/neural-style.git && \
#    pip install -r neural-style/requirements.txt && \
#    git clone https://github.com/yusuketomoto/chainer-fast-neuralstyle.git && \
#    cd chainer-fast-neuralstyle && \
#    git checkout resize-conv && \
#    pip install chainer==1.24.0 && \
#    cd ../ && \
    git clone https://github.com/lengstrom/fast-style-transfer.git && \
    pip install moviepy

ENV PYTHONPATH /app/fast-style-transfer/src

WORKDIR /app/work