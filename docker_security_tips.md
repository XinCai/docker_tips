# How to secure your Docker containers! (5 practical tips with example Dockerfiles! 🐳)

[Youtube video](https://www.youtube.com/watch?v=JE2PJbbpjsM "example video")

Tip #1: Don't run the container as the root user
example

Tip #2: Use a multi-stage build + distroless base image

Tip #3: Harden the security of the host system

Tip #4: Use a container image scanner to detect vulnerabilities

Tip #5: Don't install/configure things within the Dockerfile without understanding the potential risks


### Set current host user for docker container 

`Env: ubuntu 18.04, Docker 18.09.4, Docker-compose 1.23.2`
https://medium.com/faun/set-current-host-user-for-docker-container-4e521cef9ffc
