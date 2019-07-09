# Introduction to Docker!
Docker is a tool that allows developers to package and deploy their application into sandbox (called container) on any operating system.

# Why Docker!
Easy to containerize any complex application. It is lightweight as it runs natively on Linux and shares the kernel of the host machine. Better than virtual machine as it is not using hyperviser.

# Features!
  - Platform independent
  - Portable
  - Application Isolation
  - Light weight
  - Faster bootup
  - Swarm
  - Continuous Integration
  - Docker Hub

# Docker Image and Container!
An image is an executable package that includes everything needed to run an application-codes, a runtime, libraries, environment variables, and configuration files.
A container is a runtime instance of an image.

# Prepare your Docker environment
#### Installation
Please follow installation guide for respective operating system [here.](https://docs.docker.com/docker-for-mac/install/)

#### Check and test Docker!
```sh
$ docker info
$ docker --version
$ docker run hello-world
```

#### List Images and Containes
```sh
$ docker ps || docker container ls
$ docker ps -a || docker container ls -a
$ docker images
```

# How to create Docker Image!

We need `Dockerfile` to create docker image.
##### What is Dockerfile?

Dockerfile is some sequential sets of arguments which will be execute on docker host machine to create docker images.

Here are some basic commands for Dockerfile.
- FROM
- ENV
- WORKDIR
- RUN
- COPY
- CMD
- ENTRYPOINT

##### Example 1:

Dockerfile
```bash
FROM alpine:latest

ENTRYPOINT ["echo"]
CMD ["hello","world"]
```

##### Example 2:
Dockerfile:
```bash
FROM python:alpine

WORKDIR /

COPY Main.py start.sh /
RUN chmod +x /start.sh

ENTRYPOINT ["/start.sh"]
CMD ["hello","world"]
```

Main.py
```python
import sys

def main(args):
    print(args[0] + " " + args[1])

if __name__ == "__main__":
    main(sys.argv[1:])
```

start.sh
```bash
#!/bin/sh

python /Main.py $1 $2
```

