# Style transfer Docker images
*Docker images with set-up style transfer environments*

<br>

## Requirements
* linux box with Nvidia GPU
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
    $ docker build -t neural-style neural-style
    ```

### Running
* Enter into interactive bash
    ```bash
    $ docker run -ti --rm --runtime=nvidia neural-style bash
    ```
* Try if style transfer works with example images
    ```bash
    $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia neural-style python ../neural-style/neural_style.py --network ../imagenet-vgg-verydeep-19.mat --content ../neural-style/examples/1-content.jpg --styles ../neural-style/examples/1-style.jpg --output test.jpg --iterations 10
    ```
    This should produce `test.jpg` image in current working directory

* Run style transfer on your own images
    ```bash
    $ docker run -v "$(pwd)":/app/work -ti --rm --runtime=nvidia neural-style python ../neural-style/neural_style.py --network ../imagenet-vgg-verydeep-19.mat --content content.jpg --styles style.jpg --output test.jpg --iterations 10
    ```
    This will transfer style of `style.jpg` onto `content.jpg` resulting in `test.jpg` in current working directory

<br>

## Credits
* [neural-style](https://github.com/anishathalye/neural-style)