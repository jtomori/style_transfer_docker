# Style transfer Docker image
*Docker image with set-up style transfer environment*

<br>

## What is inside
* [neural-style](https://github.com/anishathalye/neural-style)
* [chainer-fast-neuralstyle](https://github.com/yusuketomoto/chainer-fast-neuralstyle/tree/resize-conv)
* [fast-style-transfer](https://github.com/lengstrom/fast-style-transfer)

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
* Build image
    ```bash
    $ docker build -t style-transfer .
    ```

### Running
* Enter into interactive bash
    ```bash
    $ docker run -ti --rm --runtime=nvidia style-transfer bash
    ```

* Append your style transfer command after this:
    ```bash
    $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia style-transfer
    ```
    This will mount host system working directory into `/app/work` in container. File structure in the container looks like this:
    ```
    |--- app/
         |--- work/ (working directory)
         |--- neural-style/
         |--- chainer-fast-neuralstyle/
         |--- fast-style-transfer/
         |--- imagenet-vgg-verydeep-19.mat
    ```
* For [neural-style](https://github.com/anishathalye/neural-style) commands might look like this:

    * Run style transfer on included example images
        ```bash
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia style-transfer python ../neural-style/neural_style.py --network ../imagenet-vgg-verydeep-19.mat --content ../neural-style/examples/1-content.jpg --styles ../neural-style/examples/1-style.jpg --output test.jpg --iterations 10
        ```
        This should produce `test.jpg` image in current working directory

    * Run style transfer on your own images
        ```bash
        $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia style-transfer python ../neural-style/neural_style.py --network ../imagenet-vgg-verydeep-19.mat --content content.jpg --styles style.jpg --output test.jpg --iterations 10
        ```
        This will transfer style of `style.jpg` onto `content.jpg` resulting in `test.jpg` in current working directory
