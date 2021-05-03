# A few note on Docker

* A [really helpful video by Digital Life](https://www.youtube.com/watch?v=_trJf3GbZXg) walks through the concepts of Docker.
* A nice post about the [benefits of Docker](https://towardsdatascience.com/how-docker-can-help-you-become-a-more-effective-data-scientist-7fc048ef91d5) from a fastai alum

## Terms

* **Image**: read-only template that contains a set of instructions for creating a container that can run on the Docker platform. It provides a convenient way to package up applications and preconfigured server environments, which you can use for your own private use or share publicly with other Docker users.
* **Container**: package of libraries and code that is deployable to other systems with Docker. This is the best way currently to push applications to production or make them distributable.
    * This is different than an image - a container is effectively an *app* within an image, meaning it has code, database, etc. that you would need for an app. A container is based on an image (although if the container has generic, useful libraries, it can become an image that you or the community uses)
    * This is why in Docker desktop you see two tabs - images and containers - where images are the generic setup, and containers are the apps based on these images
* **Volumes**: the persistent storage for the container. Containers on their own do not have persistent storage, so you must add a volume **when you create the container**
    * An example command is here: `sudo docker run -it --name webserver -p 80:80 -d -v ~/website:/usr/share/nginx/html nginx`. More details on this in the Tags section below
    * For example, a volume would be the top level Flutter or React directory that is created after running `flutter create myApp`

## Environments

Traditionally development in Docker was done on Linux, and therefore containers or images could easily be shared with any other Linux environment (irrespective of distribution) so long as they had Docker. This wouldn't work with Windows or Mac because containers *do not have an OS*.
* However Docker has come up with a solution: Docker Desktop. In Windows, this allows you to run Docker in the WSL environment, which since it is a Linux image, can push and pull images and containers with the Docker ecosystem

## Commands

* `sudo docker run dockerImage` - will create a **container based on this image** if downloaded, or *will download it first* if it is not available locally
    * The container name will be randomly generated. To specify the name provide a `--name` argument
    * Running this again creates a **new** container based on the same image - see next command to run an existing container

* `sudo docker start dockerContainer` - starts an **existing** container

* `sudo docker stop dockerContainer` - stops the current container

* `sudo docker rm dockerContainer` - deletes a container

* `sudo docker images` - list all containers downloaded

* `sudo docker ps -a` - show all docker containers (active and inactive)

### Tags

* `-it` - uses interactive mode for the daemon - pretty important to use
* `--name` - specify the name of the container instead of using the generated one
* `-p 80:80` - specify the port that the container is to run on in the host system
* `-d` - run in daemon mode, meaning the container will still run in the background even if the cli window is closed
* `-v someFileorDirectory` - adds a volume that **mounts** the provided directory in the container
    * Note you **must** add the volume when you create the container. If you forget, you must create a new container
    * You must also specify where in the container this directory/file (in this case `/website`) should be stored (this is added after the `:` below). For an nginx webserver, you would do the following:
        `sudo docker run --name webserver -p 80:80 -d -v ~/website:/usr/share/nginx/html nginx` 
    * You may get a forbidden error - this is because docker **doesn't have the permissions to read the file**. So do the following: `chmod 777 websites`


## Other Notes

* Images are very lightweight - you can have many on a system 
* Go to [https://hub.docker.com/](https://hub.docker.com/) to see communal images
* Containers **don't have persistent storage by default** - any changes to directories, files, state of an app will not be saved when container is stopped.


## Docker Compose
TODO: fill out when next used. COuld have used docker compose instead of 


# Creating  Dockerfile

Below assume you want to create an image (based on an image from Docker Hub).

1. First you need to create a Dockerfile. This is best explained on [Amy Tabb's blog](https://amytabb.com/ts/2020-09-19/)

```
touch Dockerfile
vim Dockerfile
cat Dockerfile
```

```
# Sample Dockerfile

FROM python:3.8

RUN useradd main-user

RUN pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio===0.7.2 -f https://download.pytorch.org/whl/torch_stable.html

RUN pip install fastai

RUN pip install jupyterlab kaggle tifffile

RUN python -m pip install opencv-python

WORKDIR /home/

# setting notebook token and password to blank is unsafe, but I need to get back to training so this
# is a temporary workaround
RUN echo '#!/bin/bash\njupyter lab --ip=0.0.0.0 --no-browser --NotebookApp.token="" --NotebookApp.password=""' >> run_jupyter.sh

WORKDIR /home

RUN chmod -R 777 /home

USER main-user

ENV HOME "/home/"

# temporary workaround for WSL2
ENV JUPYTER_RUNTIME_DIR=/tmp
```

2. Once the Dockerfile is created, run a **build** command. There are two ways:
* if you named the file *Dockerfile*: `sudo docker build -t imageName .`
* the name of Dockerfile is something else: `sudo docker build -t imageName -f alternateDockerFileName .` - notice the period at the end

3. Now the image is created locally. In order to **create a container** you need to use the **run** command: `docker run --gpus all -it --name containerName -p 8888:8888 -d -v $PWD:/home/ imageName`. Things to note about this command:
* You **must include the `--gpus all` flag**
* You need to **mount a volume** using `-v directoryToMount:hostDirectory`, where `directoryToMound` *cannot have spaces* and can be `$PWD` if you run this command from that directory, and where `hostDirectory` is typically home
* You can specify the `-w` command, which is the directory you are loaded into by default. However we have covered this in the Dockerfile using `WORKDIR`
* `-d` and `-it` are critical. Without them, the daemon wouldn't run and it won't stay "interactive"

4. Once the container is up and running, you can bash into the container using **exec**: `docker exec -it containerName bash`. If however there is another command you want to run, just replace bash: `docker exec -it containerName jupyter lab --ip=0.0.0.0 --no-browser --NotebookApp.token="" --NotebookApp.password=""`
    * You can also bypass the token with the following line `docker logs containerName | grep token | cut -d '=' -f2 | head -n 1`


## Permissions 

### Linux

See [this great answer](https://www.digitalocean.com/community/questions/how-to-fix-docker-got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socket) if you run up against a permission error in Linux


## Custom (Derivative) Image for GCP

Follow [these instructions](https://cloud.google.com/ai-platform/deep-learning-containers/docs/derivative-container) by Google on creating a derivative container. Note they recommend copying one of their images and making modifications

### Sample GCP Modified Container

Dockerfile for derivative container
```
# Pytorch 1.8
FROM gcr.io/deeplearning-platform-release/pytorch-gpu

RUN pip install -U jupyterlab

RUN useradd -ms /bin/bash craig

RUN chmod -R 777 /home

USER craig

WORKDIR /home/jupyter

```

Once the above Dockerfile is created, need to run the below to have access to a `gcr` link for custom image setup

```
docker build . -f Dockerfile.example -t "gcr.io/{PROJECT-ID}/{CONTAINER-NAME}:latest"
docker push "gcr.io/{PROJECT-ID}/{CONTAINER-NAME}:latest"
```
So far this seems to work

## Python Development Environment

As part of the OpenMined course I am doing, I need a dev environment. Unfortunately the new M1 Macbook isn't too friendly yet to certain packages, and so a Docker development environment is a decent choise since Docker Desktop is in GA for ARM-based chips.

According to [this post](https://pythonspeed.com/articles/base-image-python-docker-images/), all we need is the offical Python image as the base - the rest is overkill (we don't need a full Linux operating system, and we don't need a tonne of packes installed).

The initial Dockerfile for this image is the following:

```
FROM python:3.9.4-slim

RUN apt-get update \
&& apt-get install gcc -y \
&& apt-get clean \
&& apt-get install -y build-essential \
&& apt-get -y install git

RUN apt install -y libavdevice-dev libavfilter-dev libopus-dev libvpx-dev pkg-config libsrtp2-dev

RUN git clone https://github.com/OpenMined/courses.git --branch foundations-of-private-computation --single-branch

RUN pip install -r courses/requirements.txt
```
As you can see, we use the offical Python image, a few standard updates (including **installing git** - notice the `-y` flag), fetching the Openmined library and installing the packages from `requirements.txt`.

After creating this Dockerfile (via `touch Dockerfile` or `vim Dockerfile`), we:
1. Run `docker build -t openmined .`
2. Enter `docker run -it -d -p 5000:5000 openmined`
   * Note the flags are all key here, including specifying the port
3. Run 

**Need to update this with the fact we use `CMD` to trigger jupyter lab startup**
**Also need to recognise that the port number in the docker container is the one that communicates with the port on the host machine - therefore if the ports are different, you must use the local host machine port to access the container**
