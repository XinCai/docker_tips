# How to secure your Docker containers! (5 practical tips with example Dockerfiles! 🐳)

[Youtube video](https://www.youtube.com/watch?v=JE2PJbbpjsM "example video")

### Tip #1: Don't run the container as the root user

[non-root Dockerfile](Dockerfile.non-root "example")

### Tip #2: Use a multi-stage build + distroless base image

[Distroless Docker](https://www.youtube.com/watch?v=lviLZFciDv4 "2017 swampUP Sessions | Distroless Docker: Containerizing Apps, not VMs - Matthew Moore")

```
Distroless：谷歌内部使用的镜像构建文件

Distroless 是谷歌内部使用的镜像构建文件，包括 Java 镜像，Node，Python 等镜像构建文件，Distroless 仅仅只包含运行服务所需要的最小镜像，不包含包管理工具，shell 命令行等其他功能。
```

[Docker MultiStage build](https://docs.docker.com/develop/develop-images/multistage-build/ "multi stage")

[MultiStage Dockerfile](Dockerfile.distroless "example")

### Tip #3: Harden the security of the host system

### Tip #4: Use a container image scanner to detect vulnerabilities

### Tip #5: Don't install/configure things within the Dockerfile without understanding the potential risks


### Set current host user for docker container 

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

