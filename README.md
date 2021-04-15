# Container (docker) Tips


[Container Security -- By Liz Rice (OReilly)](https://github.com/XinCai/docker_tips/blob/09d335ce35417d34ca9df6b7fc89a3ad2b04a65c/Container%20Security%20by%20Liz%20Rice%20-%20OReilly%20Apr%202020.pdf "book")

## docker container 安全考虑类别 和 Tips

#### 1.Vulnerable application code

The best way to avoid running containers with known vulunerabilities is to scan images 
扫描 docker images, 持续扫描 scanning process application 是否使用了 out of date packages （考虑使用snyk ， 来持续scan application code package）

#### 2. Badly configured container images
配置 container 的时候注意事项 configuring container to run as the root user

#### 3. Build machine attacks

#### 4. Supply chain attacks 

images stored in a registry, 当用户 pull images 时候，确定 pulled 的image 是 当时 pushed 的images.

#### 5. Badly configured containers

#### 6. Vulnerable hosts
容器运行在host machine上面，确定 hosts上 没有运行 vulnerable code, 好的方法是 确定 minimize the amount of software installed on each host.

#### 7. Exposed secrets
Application code 需要配置 密码 或 证书 一类的来 communicate with other components in a system. 

#### 8. Insecure networking
不安全的网络， container 需要 通信 其他的 containers,  或者 outside world


## 安全边界 （security boundaries）
1. 容器是一个security boundary, 运行在container 内的 代码 不应该访问 容器外部的code or data 
2. 数量越多的 security boundaries，安全性越高， 加强每一层的 boundary, 安全性越高

## 多租户 Multitenancy 

共享计算资源 （sharing machine resources） 被成为 `multitenancy`
在一个多租户的环境，不同的users (或者成为 tenants) 运行他们的 workloads 在共享的硬件（或者计算资源）， 需要有 更强的 boundaries between 他们的 workloads, 来确保之间不会被干扰。

### Virtualization

虚拟机 VM -- 有操作系统的虚拟机，通常来讲，虚拟机之间有强力的隔离效果， 意思是 邻居 （neighbors）大多不会 观察到或者 干扰到其他的 租户 (tenant)在 VM. 
多租户 (multitenancy): 意思是 多个不同的group people 共享一个软件  (share a single instance of same software)， 多租户是指软件架构支持一个实例服务多个用户（Customer），每一个用户被称之为租户（tenant），软件给予租户可以对系统进行部分定制的功能。

```
例子：
操作系统 --- 单租户系统
电子邮件 --- 多租户系统
```

### Container Multitenancy 
容器的多租户
1. 在 kubernetes 世界里，使用 `namespace` 来细分 a cluster of machines 机器集群, 被不同的团队，个人和软件来使用。
2. 使用 RBAC （role based access control）来限制 people 和  components 来访问 kubernetes的 不同namespaces


## 安全原则 （security principal）

#### 1. Least Privilege （最低权限 原则）
最低权限原则， 对个人 或者 一个组成员 给出最低的权限， 不超过其职能范围的访问权限。

#### 2. Defense in Depth (深度防御 原则)
多层面 protection， layers of protection.
一个layer 出现了安全状况， 另一个layer 会保护你的部署。 

#### 3. Reducing the Attack Surface （减少攻击面 原则）

1. Reducing access points by keeping interfaces small and simple where possible
2. Limiting the users and components who can access a service
3. Minimizing the amount of code

#### 4. Limiting the Blast Radius （限制爆炸半径 原则）

分割安全控制 表示 当出现 安全事件时，影响是有限的。容器非常适合遵循这一项原则， 因为将架构划分为多个微小的实例服务 micro service， 容器本身就可以充当安全边界。

#### 5. Segregation of Duties （指责分工）
根据人员的职责 来给于特定的 权限。


## File Permissions (文件权限是 安全的基石)  
在 linux 操作系统下，everything is a file. 


