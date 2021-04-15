# Container (docker) Tips


[Container Security -- By Liz Rice (OReilly)](https://github.com/XinCai/docker_tips/blob/09d335ce35417d34ca9df6b7fc89a3ad2b04a65c/Container%20Security%20by%20Liz%20Rice%20-%20OReilly%20Apr%202020.pdf "book")

## docker container 安全考虑类别 和 Tips

#### Vulnerable application code

The best way to avoid running containers with known vulunerabilities is to scan images 
扫描 docker images, 持续扫描 scanning process application 是否使用了 out of date packages （考虑使用snyk ， 来持续scan application code package）

#### Badly configured container images
配置 container 的时候注意事项 configuring container to run as the root user

#### Build machine attacks

#### Supply chain attacks 

images stored in a registry, 当用户 pull images 时候，确定 pulled 的image 是 当时 pushed 的images.

#### Badly configured containers

#### Vulnerable hosts
容器运行在host machine上面，确定 hosts上 没有运行 vulnerable code, 好的方法是 确定 minimize the amount of software installed on each host.

#### Exposed secrets
Application code 需要配置 密码 或 证书 一类的来 communicate with other components in a system. 

#### Insecure networking
不安全的网络， container 需要 通信 其他的 containers,  或者 outside world


### 安全边界 （security boundaries）
1. 容器是一个security boundary, 运行在container 内的 代码 不应该访问 容器外部的code or data 
2. 数量越多的 security boundaries，安全性越高， 加强每一层的 boundary, 安全性越高

### 多租户 Multitenancy 

共享计算资源 （sharing machine resources） 被成为 `multitenancy`
在一个多租户的环境，不同的users (或者成为 tenants) 运行他们的 workloads 在共享的硬件（或者计算资源）， 需要有 更强的 boundaries between 他们的 workloads, 来确保之间不会被干扰。






