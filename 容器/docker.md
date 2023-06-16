## Docker

### 一些链接

官网
https://www.docker.com/

DockerInfo
http://www.dockerinfo.net/document

Docker-从入门到实战
https://yeasy.gitbook.io/docker_practice/


### 概述

- 开源项目
- Go语言实现
- 技术方案：基于Linux内核的cgroup、namespace
- 一个用于开发、交付和运行应用程序的开放平台
- 目标是实现轻量化的操作系统虚拟化解决方案

优势
- 实现快速交付
- 减少代码编写和部署运行代码之间的延迟
- 基础架构管理方便

应用场景
- 快速、一致地交付应用程序，适合持续集成和持续交付（CI/CD）工作流
- 响应式部署和扩展
- 在相同的硬件上运行更多的工作负载

架构
- 客户端-服务器（CS）架构
- Docker守护进程（dockerd）：侦听Docker API请求并管理Docker对象，如图像、容器、网络和卷
- Docker客户端（docker）：是Docker用户和Docker交互的主要方式
- Docker注册表：存储Docker镜像，Docker Hub是任何人都可以使用的公共注册表
- Docker对象：镜像、容器、网络、卷等都是对象
- 镜像：只读模板，特殊的文件系统
- 容器：镜像的可运行实例

