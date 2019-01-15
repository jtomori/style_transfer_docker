# Style transfer Docker images
*Docker images with set-up style transfer environments*

<br>

## What is inside
This repository contains two images:

1. `neural-fast-style-transfer` containing:
    * [neural-style](https://github.com/anishathalye/neural-style)
    * [fast-style-transfer](https://github.com/lengstrom/fast-style-transfer)
    * *pre-trained VGG 19* network required by both repos
    * [example models](https://drive.google.com/drive/folders/0B9jhaT37ydSyRk9UX0wwX3BpMzQ) for **fast-style-transfer**

2. `chainer-style-transfer` containing:
    * [chainer-fast-neuralstyle](https://github.com/yusuketomoto/chainer-fast-neuralstyle/tree/resize-conv)
    * *pre-trained VGG 16* network required for training

* the following files are **not** included:
    * [coco2014 dataset](http://msvocds.blob.core.windows.net/coco2014/train2014.zip) needed for training new models for **fast-style-transfer** and **chainer-fast-neuralstyle**

<br>

## Requirements
* linux box with a Nvidia GPU
* [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) and [nvidia-docker](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0)) installed

<br>

## Running
* Entering into interactive bash
    ```bash
    $ docker run -ti --rm --runtime=nvidia jtomori/neural-fast-style-transfer bash
    ```
    ```bash
    $ docker run -ti --rm --runtime=nvidia jtomori/chainer-style-transfer bash
    ```


* Command structure: append your style transfer command at the end:
    ```bash
    $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia jtomori/neural-fast-style-transfer [your command here]
    ```
    This will mount host system working directory into `/app/work` in container. File structure in the container looks like this in `neural-fast-style-transfer`
    ```
    |--- app/
         |--- work/ (working directory)
         |--- neural-style/
         |--- fast-style-transfer/
         |--- imagenet-vgg-verydeep-19.mat
    ```
    and like this in `chainer-style-transfer`
    ```
    |--- app/
         |--- work/ (working directory)
         |--- chainer-fast-neuralstyle/
    ```
* For [neural-style](https://github.com/anishathalye/neural-style) commands might look like this:

    * Run style transfer on included example images
        ```bash
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia jtomori/neural-fast-style-transfer python ../neural-style/neural_style.py --network ../imagenet-vgg-verydeep-19.mat --content ../neural-style/examples/1-content.jpg --styles ../neural-style/examples/1-style.jpg --output test.jpg --iterations 10
        ```
        This should produce `test.jpg` image in current working directory

    * Run style transfer on your own images
        ```bash
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia jtomori/neural-fast-style-transfer python ../neural-style/neural_style.py --network ../imagenet-vgg-verydeep-19.mat --content content.jpg --styles style.jpg --output test.jpg --iterations 10
        ```
        This will transfer style of `style.jpg` onto `content.jpg` resulting in `test.jpg` in current host working directory.

* Command for [fast-style-transfer](https://github.com/lengstrom/fast-style-transfer):

    * Run style transfer on included example image with model `la_muse.ckpt`
        ```
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia jtomori/neural-fast-style-transfer python ../fast-style-transfer/evaluate.py --checkpoint ../fast-style-transfer/examples/models/la_muse.ckpt --in-path ../fast-style-transfer/examples/content/chicago.jpg --out-path test.jpg
        ```
        This will transfer style of example model `la_muse.ckpt` on example image provided with the repository resulting in `test.jpg`. You can replace `../fast-style-transfer/examples/content/chicago.jpg` with `content.jpg` to apply style on `content.jpg` in your current host directory.

        Included example models:
        * `la_muse.ckpt`
        * `rain_princess.ckpt`
        * `scream.ckpt`
        * `udnie.ckpt`
        * `wave.ckpt`
        * `wreck.ckpt`
    
    * Train a new model based on `style.jpg`
        ```bash
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia jtomori/neural-fast-style-transfer python ../fast-style-transfer/style.py --vgg-path ../imagenet-vgg-verydeep-19.mat --style style.jpg --checkpoint-dir model --train-path dataset
        ```
        Note that you need to pass path to [coco2014](http://msvocds.blob.core.windows.net/coco2014/train2014.zip) dataset, which is not included in the Docker image. In this case it is `dataset` folder in host working directory. `model` directory also needs to be created there.

* Command for [chainer-fast-neuralstyle](https://github.com/yusuketomoto/chainer-fast-neuralstyle/tree/resize-conv):

    * Run style transfer on included example image and model, which will result in `test.jpg` in your curent host working directory
        ```bash
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia jtomori/chainer-style-transfer python3 ../chainer-fast-neuralstyle/generate.py ../chainer-fast-neuralstyle/sample_images/tubingen.jpg -m ../chainer-fast-neuralstyle/models/composition.model -o test.jpg -g 0
        ```
        
        Included example models:
        * `composition.model`
        * `seurat.model`
        
        ***Note:** You can use `--memory 7000m` optional argument, which limits containers memory usage. Set it accordingly to your free memory if you encounter system crashes.*
    
    * Train a new model
        ```bash
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia jtomori/chainer-style-transfer python3 ../chainer-fast-neuralstyle/train.py --vggmodel ../chainer-fast-neuralstyle/vgg16.model --dataset dataset/ --style_image style.jpg --output model/style -g 0
        ```
        This will generate a new model `model/style.model` based on `style.jpg` in a current host working directory. `dataset` folder also needs to be present there.

<br>

## Building locally
This step is required only if you want to make modifications to the image and to do a local build.
* Clone this repo and enter it
    ```bash
    $ git clone https://github.com/jtomori/style_transfer_docker.git
    $ cd style_transfer_docker
    ```
* Build images
    1. `neural-fast-style-transfer`
        ```bash
        $ docker build -t neural-fast-style-transfer neural-fast-style-transfer
        ```
    2. `chainer-style-transfer`
        ```bash
        $ docker build -t chainer-style-transfer chainer-style-transfer
        ```
