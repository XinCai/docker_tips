# How to secure your Docker containers! (5 practical tips with example Dockerfiles! 🐳)

[Youtube video](https://www.youtube.com/watch?v=JE2PJbbpjsM "example video")

### Tip #1: Don't run the container as the root user

参考Dockerfile定义， `line 2` [non-root Dockerfile](Dockerfile.non-root "example") 

在这个dockerfile 使用的base image 里面， 定义了 `user：node` 和 `user id: 1000`

[参考 node:12-slim 镜像的定义](https://github.com/nodejs/docker-node/blob/31246f5f779cafa0930a1db04bd00d875d6a940d/12/stretch-slim/Dockerfile "定义了基础image的 user")

```
RUN groupadd --gid 1000 node \
  && useradd --uid 1000 --gid node --shell /bin/bash --create-home node
```

所以在[non-root Dockerfile](Dockerfile.non-root "example")文件中使用这个基础 image 时候可以 set User to Non-Root `User 1000` -- best practice


### Tip #2: Use a multi-stage build + distroless base image

**参考这个 Dockerfile, multi-stage build**

好处： 
1. Keeping the image size down. 

Tips: 
1. Compresses two `RUN` commands together using the Bash `&&` operator, to avoid creating an additional layer in the image
For Example
```
FROM golang:1.7.3
WORKDIR /go/src/github.com/alexellis/href-counter/
COPY app.go .
RUN go get -d -v golang.org/x/net/html \
  && CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .
``` 

```
### First Stage ###
# Base Image
FROM node:12-slim as build
WORKDIR /usr/src/app

# Install Dependencies
COPY package*.json ./
RUN npm install

# Copy in Application
COPY . .

### Second Stage ###
FROM gcr.io/distroless/nodejs:12

# Copy App + Dependencies from Build Stage
COPY --from=build /usr/src/app /usr/src/app
WORKDIR /usr/src/app

# Set User to Non-Root
USER 1000

# Run Server
CMD [ "server.js" ]
```

[Distroless Docker](https://www.youtube.com/watch?v=lviLZFciDv4 "2017 swampUP Sessions | Distroless Docker: Containerizing Apps, not VMs - Matthew Moore")

```
Distroless：谷歌内部使用的镜像构建文件

Distroless 是谷歌内部使用的镜像构建文件，包括 Java 镜像，Node，Python 等镜像构建文件，Distroless 仅仅只包含运行服务所需要的最小镜像，
不包含包管理工具，shell 命令行等其他功能。

"Distroless" images contain only your application and its runtime dependencies. They do not contain package managers, 
shells or any other programs you would expect to find in a standard Linux distribution.
```

[Docker MultiStage build](https://docs.docker.com/develop/develop-images/multistage-build/ "multi stage")

[MultiStage Dockerfile](Dockerfile.distroless "example")

### Tip #3: Harden the security of the host system

### Tip #4: Use a container image scanner to detect vulnerabilities

### Tip #5: Don't install/configure things within the Dockerfile without understanding the potential risks


## Set current host user for docker container (Example)

`Env: ubuntu 18.04, Docker 18.09.4, Docker-compose 1.23.2`

https://medium.com/faun/set-current-host-user-for-docker-container-4e521cef9ffc

#### Step1: Create Dockerfile
```
# Dockerfile
ARG DOCKER_BASE_IMAGE=<BASE IMAGE NAME>
FROM $DOCKER_BASE_IMAGE
ARG USER=docker
ARG UID=1000
ARG GID=1000
# default password for user
ARG PW=docker
# Option1: Using unencrypted password/ specifying password
RUN useradd -m ${USER} --uid=${UID} && echo "${USER}:${PW}" | \
      chpasswd
# Option2: Using the same encrypted password as host
#COPY /etc/group /etc/group 
#COPY /etc/passwd /etc/passwd
#COPY /etc/shadow /etc/shadow
# Setup default user, when enter docker container
USER ${UID}:${GID}
WORKDIR /home/${USER}
```

#### Step2: Build Image 
```
# bash
export UID=$(id -u)
export GID=$(id -g)
docker build --build-arg USER=$USER \
             --build-arg UID=$UID \
             --build-arg GID=$GID \
             --build-arg PW=<PASSWORD IN CONTAINER> \
             -t <IMAGE NAME> \
             -f <DOCKERFILE NAME>\
             .
```

