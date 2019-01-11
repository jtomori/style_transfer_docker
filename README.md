# Style transfer Docker images
*Docker images with set-up style transfer environments*

<br>

## What is inside
Because of dependencies issues this repository contains two images:

1. `neural-fast-style-transfer/Dockerfile` containing:
    * [neural-style](https://github.com/anishathalye/neural-style)
    * [fast-style-transfer](https://github.com/lengstrom/fast-style-transfer)
    * *pre-trained VGG 19* network required by both repos
    * the following files required by **fast-style-transfer** are **not** included:
        * [models](https://drive.google.com/drive/folders/0B9jhaT37ydSyRk9UX0wwX3BpMzQ?usp=sharing) for applying styles
        * [coco2014](http://msvocds.blob.core.windows.net/coco2014/train2014.zip) for training new styles

2. `chainer-style-transfer/Dockerfile` containing:
    * [chainer-fast-neuralstyle](https://github.com/yusuketomoto/chainer-fast-neuralstyle/tree/resize-conv)

<br>

## Requirements
* linux box with a Nvidia GPU
* [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) and [nvidia-docker](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0)) installed

<br>

## Usage

### Building
* Clone this repo
    ```bash
    $ git clone https://github.com/jtomori/style_transfer_docker.git
    ```
* Build images
    1. `neural-fast-style-transfer`
        ```bash
        $ docker build -t neural-fast-style-transfer neural-fast-style-transfer
        ```

### Running
* Entering into interactive bash
    ```bash
    $ docker run -ti --rm --runtime=nvidia neural-fast-style-transfer bash
    ```

* Command structure: append your style transfer command after this:
    ```bash
    $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia neural-fast-style-transfer
    ```
    This will mount host system working directory into `/app/work` in container. File structure in the container looks like this:
    ```
    |--- app/
         |--- work/ (working directory)
         |--- neural-style/
         |--- fast-style-transfer/
         |--- imagenet-vgg-verydeep-19.mat
    ```
* For [neural-style](https://github.com/anishathalye/neural-style) commands might look like this:

    * Run style transfer on included example images
        ```bash
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia neural-fast-style-transfer python ../neural-style/neural_style.py --network ../imagenet-vgg-verydeep-19.mat --content ../neural-style/examples/1-content.jpg --styles ../neural-style/examples/1-style.jpg --output test.jpg --iterations 10
        ```
        This should produce `test.jpg` image in current working directory

    * Run style transfer on your own images
        ```bash
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia neural-fast-style-transfer python ../neural-style/neural_style.py --network ../imagenet-vgg-verydeep-19.mat --content content.jpg --styles style.jpg --output test.jpg --iterations 10
        ```
        This will transfer style of `style.jpg` onto `content.jpg` resulting in `test.jpg` in current host working directory.

* Command for [fast-style-transfer](https://github.com/lengstrom/fast-style-transfer):

    * Run style transfer on included example image with model `la_muse.ckpt` downloaded in working directory
        ```
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia neural-fast-style-transfer python ../fast-style-transfer/evaluate.py --checkpoint la_muse.ckpt --in-path ../fast-style-transfer/examples/content/chicago.jpg --out-path test.jpg
        ```
        This will transfer style of `la_muse.ckpt` on example image provided with the repository resulting in `test.jpg`. You can replace `../fast-style-transfer/examples/content/chicago.jpg` with `content.jpg` to apply style on `content.jpg` in your current host directory.