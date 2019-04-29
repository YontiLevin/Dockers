# Deep Learning One Stop Shop

### Specs
ubuntu 16.04
cuda 10.0
python 3.6.7
pytorch 1.0.1.post2
tensorflow 1.13.1
jupyter with r kernel support
many more python packages...
many r packages...
openssh and other linux tools...

### Installation
1. make sure your host has the nvidia-docker installed and the right cuda support
2. run from inside the folder
```
docker build -t <docker_name> .
```
3. run the image using 
```
nvidia-docker run ...
```
