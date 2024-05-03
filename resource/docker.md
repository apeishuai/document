docker是一种虚拟化手段，通过重新组织计算机资源，并提供一套管理系统，来达到摆脱具体的计算机硬件的目的\
docker提供一套文件系统，所以其具有原生Linux调用资源的速度\
[[https://hub.docker.com/search?q=][dockerhub]]


# 安装
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
curl -sSL https://get.daocloud.io/docker | sh

# 架构


# 使用
环境构建：
操作系统\
网络\

```bash
Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Common Commands:
  run         Create and run a new container from an image
  exec        Execute a command in a running container
  ps          List containers
  build       Build an image from a Dockerfile
  pull        Download an image from a registry
  push        Upload an image to a registry
  images      List images
  login       Log in to a registry
  logout      Log out from a registry
  search      Search Docker Hub for images
  version     Show the Docker version information
  info        Display system-wide information

Management Commands:
  builder     Manage builds
  buildx*     Docker Buildx (Docker Inc., v0.13.0)
  checkpoint  Manage checkpoints
  compose*    Docker Compose (Docker Inc., v2.24.7)
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  plugin      Manage plugins
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Swarm Commands:
  config      Manage Swarm configs
  node        Manage Swarm nodes
  secret      Manage Swarm secrets
  service     Manage Swarm services
  stack       Manage Swarm stacks
  swarm       Manage Swarm

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  wait        Block until one or more containers stop, then print their exit codes

Global Options:
      --config string      Location of client config files (default "/home/shuai/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides
                           DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket to connect to
  -l, --log-level string   Set the logging level ("debug", "info", "warn", "error", "fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "/home/shuai/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/home/shuai/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/home/shuai/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Run 'docker COMMAND --help' for more information on a command.
```

## 容器
```bash
    docker ps
    docker ps -a
    docker run [选项] [镜像名] [命令]
    docker start/stop/rm [容器id]

    docker export 
    docker inspect <container_id>
```
## image
## volume

```bash
docker [OPTIONS] command

<<run>>
docker run
    docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
    OPTION
        -i interactive
	-t allocate a pseudo-TTY
	-v bind mount a volume
	-d detach (run container in background and print container)
	--rm 

    mine:
      docker run ubuntu:15.10 /bin/echo "hello world"
      sudo docker run -itd --rm --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
      sudo docker run --rm -ti -v /home/shuai/apeishuai:/nikola dragas/nikola nikola init

      docker run <coinxx-id> -it bin/bash

docker安装的bash脚本放在了虚拟机
```
```bash
>image
    [[g:/emacs-notes/Tool/inbox/Docker.org::build][build]]
        通过Dockerfile构建docker镜像
	Dockerfile 的指令每执行一次都会在 docker 上新建一层。
    [[g:/emacs-notes/Tool/inbox/Docker.org::run][run]]
    docker system/image/container prune 清理未使用过的镜像、容器、网络和卷

    docker logs [容器id]
    docker inspect [容器id]

>xx
    docker network inspect [网络id]

    docker cp [] []:[]
    docker exec -it [容器id] [命令]

>build

>

composed
exec
ps
pull
images
login
logout
search
version
info
image
volume
compose*
builder
```

# 网络(容器互访)
```bash
[[https://www.cnblogs.com/shenh/p/9714547.html][Docker容器互访三种方式 - 三只松鼠 - 博客园]]\
/Entered on/ [2023-04-24 周一 15:27]

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks


Usage:  docker network create [OPTIONS] NETWORK

Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by
                             Network driver (default map[])
      --config-from string   The network from which to copy the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a
                             network segment

bridge 默认，每个容器都会获得一个独立ip
host 使用宿主机的网络堆栈
overlay 创建一个覆盖网络，用于docker swarm集群
macvaln 创建一个MAC虚拟局域网，每个容器都会获得一个独立的MAC地址
none  创建一个没有任何网络接口的网络
transparent 创建一个透明模式的网络，通常与第三方网络插件一块用


docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  my_macvlan_network
	
docker run -it --name --network <vridge> --network-alias <><>
```


# xxx
## 本地image构建
docker build -t your_image_name .\

[[https://zhuanlan.zhihu.com/p/34488175][使用 Docker 构建自己的镜像 - 知乎]]\
/Entered on/ [2023-04-24 周一 15:29]

config: <<build>>
FROM centos
RUN yum -y yum install wget \
    && wget -O redis.tar.gz
    && tar -xvf redis.tar.gz

cmd:
docker build -tn nginx:v3


## 数据保存
-v <host_path>:<container_path>\
-p <host_port>:<container_port>\

sudo docker run --rm -ti -v /yourpath:/nikola dragas/nikola nikola init\

sudo docker list


# 配置
## dockerfile
```bash
from xx

workdir /app

run apt-get update && \
    apt-get install -y python3 && \
    apt-get clean

env lang c.utf-8

cmd ["python3","app.py"]


env  set environment variables
from create a new build stage from a base image
label add metadata to an image
run execute build commands
volume create volume mounts
workdir change working directory

https://docs.docker.com/reference/dockerfile/#overview
```

## docker-compose
```bash
version: '3'

service:
  web:
  db:
      port
      depends_on
      environment
      volume
networks:
  mynetwork:
    external: true
```
