# Introduction to Docker!
Docker is a tool that allows developers to package and deploy their application into sandbox (called container) on any operating system.

## Why Docker!
Easy to containerize any application. It is lightweight as it runs natively on Linux and shares the kernel of the host machine. Better than virtual machine as it is not using hyperviser.

## Features!
  - Platform independent
  - Portable
  - Application Isolation
  - Light weight
  - Faster bootup
  - Swarm
  - Continuous Integration
  - Docker Hub

## Docker Image and Container!
An image is an executable package that includes everything needed to run an application-codes, a runtime, libraries, environment variables, and configuration files.
A container is a runtime instance of an image.

## Prepare your Docker environment
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

## How to create Docker Image!

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
```dockerfile
FROM alpine:latest

ENTRYPOINT ["echo"]
CMD ["hello","world"]
```

##### Example 2:
Dockerfile:
```dockerfile
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

## Docker Compose!

Compose is a tool for defining and running multi-container Docker applications. 
With Compose, we use a YAML file to configure your application’s services and with single command, we can create and 
start all services with configuration.

#### Compose is basically a three-step process.

- Define `Dockerfile`
- Define `docker-compose.yml`
- Run `docker-compose up`

Example :

We can use `Dockerfile` present in `demo3` folder and we are creating compose file as below.

docker-compose.yml
```bash
version: '3'

services:
  my_app:
    build:
      context: ./demo3
      dockerfile: Dockerfile
    image: test:3
    ports:
      - 8088:7777
```

Run:
```bash
docker-compose up
```
Service Url : [127.0.0.1:8088](http://127.0.0.1:8088)

## Docker Swarm!
Docker Swarm is a clustering and scheduling tool for Docker containers. With Swarm, IT administrators and developers can establish and manage a cluster of Docker nodes as a single virtual system. 

Let's create docker swarm. You need to follow below commands.

Windows:
```bash
docker-machine create --driver hyperv master1
docker-machine create --driver hyperv worker1
docker-machine create --driver hyperv worker2
docker-machine create --driver hyperv worker3
```

macOS or linux:
```bash
docker-machine create --driver virtualbox master1
docker-machine create --driver virtualbox worker1
docker-machine create --driver virtualbox worker2
docker-machine create --driver virtualbox worker3
```

Find ip of master or worker machine
```bash
 docker-machine ip <machine-name>
```

SSH on node :
```bash
docker-machine ssh <machine-name>
```

Swarm initialization :
```bash
docker swarm init --advertise-addr MASTER_IP
```

List nodes on master :
```bash
docker node ls
```

Add workers to swarm :
```bash
docker swarm join-token manager
```
Note : you will get docker command with token. you need to execute in each workers to add them in swarm.

Now create service in swarm :
```bash
docker service create --replicas 3 -p 80:80 --name web nginx:latest
docker service ls
docker service inspect web
docker service ps web
```
Note : you can access this nginx service on each node's ip.

Scaling up and Scaling down :
```bash
docker service scale web=5
docker service ps web
```

Inspecting nodes :
```bash
docker node inspect — pretty self
docker node inspect — pretty worker1
docker node inspect — pretty worker2
docker node inspect — pretty worker3
```

Draining a node :
```bash
docker node update --availability drain worker1
docker node update --availability drain worker3
```

Applying Rolling Updates :
```bash
docker service update --image <imagename>:<version> web
```

Remove the Service :
```bash
docker service rm web
docker service ls
docker service inspect web
```

Stop and remove machine :
```bash
docker-machine stop master
docker-machine stop worker1
docker-machine stop worker2
docker-machine stop worker3

docker-machine rm master
docker-machine rm worker1
docker-machine rm worker2
docker-machine rm worker3
```