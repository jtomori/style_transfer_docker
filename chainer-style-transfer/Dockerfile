FROM chainer/chainer:v5.1.0-python3

# Install packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    curl && \
    apt-get autoclean && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Clone repo, install requirements
RUN git clone --single-branch --branch resize-conv https://github.com/jtomori/chainer-fast-neuralstyle.git && \
    cd chainer-fast-neuralstyle && \
    pip3 install Pillow && \
    curl http://www.robots.ox.ac.uk/~vgg/software/very_deep/caffe/VGG_ILSVRC_16_layers.caffemodel --output VGG_ILSVRC_16_layers.caffemodel && \
    python3 create_chainer_model.py && \
    rm VGG_ILSVRC_16_layers.caffemodel

WORKDIR /app/work