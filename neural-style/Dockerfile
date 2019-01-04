FROM tensorflow/tensorflow:latest-gpu-py3
# TODO - do not run as root

# Install packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git && \
    apt-get autoclean && \
    apt-get clean && \
    apt-get autoremove

WORKDIR /app

# Download pre-trained VGG network
# TODO - move into multi-stage builds? might save space if needed in multiple images
RUN curl http://www.vlfeat.org/matconvnet/models/imagenet-vgg-verydeep-19.mat --output imagenet-vgg-verydeep-19.mat

# Clone repo, install requirements
RUN mkdir work && \
    git clone https://github.com/anishathalye/neural-style.git && \
    cd neural-style && \
    pip install -r requirements.txt

WORKDIR /app/work