
/**********************************************Docker安装*******************************************
 ***************************************************************************************************/

1、卸载旧版本docker
	sudo apt-get remove docker docker-engine docker-ce docker.io

2、更新ubuntu的apt源索引
	sudo apt-get update

3、配置安装包允许apt通过HTTPS使用仓库
	sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

4、添加Docker官方GPG key
	4.1、curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	4.2、sudo apt-key fingerprint 0EBFCD88

5、设置Docker稳定版仓库
	sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"

6、再次更新apt源索引
	sudo apt-get update

7、安装最新版Docker CE（社区版）
	sudo apt-get install docker-ce

{

如果要安装指定版本的docker按如下操作（不需要也可以跳过这步操作）
	$ apt-cache madison docker-ce    # 列出可用的docker-ce版本
	$ sudo apt-get install docker-ce=18.06.1~ce~3-0~ubuntu    #安装指定的docker版本
}

8、拉取hello-world镜像测试docker容器
	sudo docker run hello-world

9、可使用docker -v查看docker版本


二、安装 nvidia-docker	（网址：https://blog.csdn.net/m0_37167788/article/details/104824390）

1、执行下 命令
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \ 
	sudo apt-key add -

$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list

$ sudo apt-get update

2、安装nvidia-docker
	sudo apt-get install -y nvidia-docker2


3、安装一个docker容器进行测试
	sudo pkill -SIGHUP dockerd

	sudo docker run --runtime=nvidia --rm nvidia/cuda:10.1-base nvidia-smi


/*-----------------------------------zmkg-proV2.2.tar镜像使用说明-------------------------------×/
1、 镜像导入
	在zmkg-proV2.2.tar目录下执行：
	$ sudo docker  load  --input  zmkg-proV2.2.tar


2、 构建配置目录
	$ mkdir   ~/zmkg-pro

3、 拷贝配置
	$ cp -r config ~/zmkg-pro

4、 创建容器
	$ sudo docker run  --runtime nvidia -it  --privileged -v /dev:/dev  -p 8088:8088 -v ~/zmkg-pro/config:/home/zmkg-pro/config zmkg-pro:v2.2 /bin/bash

5、 拉取视频流
流地址:
rtmp://主机IP:8088/live/stream1
rtmp://主机IP:8088/live/stream2
rtmp://主机IP:8088/live/stream3
rtmp://主机IP:8088/live/stream4
rtmp://主机IP:8088/live/stream5

注意:
目前配置里的数据源地址为测试视频,如果要接入摄像头则需要修改数据源地址.
如:
配置原来测试数据源stream_addr=config/cut2.mp4
修改后变为stream_addr=/dev/video0
其它源地址类似修改即可

/*------------------------------------------------------------------------------------------*/

Docker运行镜像时问题和解决：
一、nvidia-docker 数据流出现“Bind for 0.0.0.0:8088 failed: port is already allocated.”
	解决：
	1、$ sudo docker ps		//查看进程，发现相关的容器并没有在运行，而 docker-proxy 却依然绑定着端口：
	2、$ ps -aux | grep -v grep | grep docker-proxy		//检查docker镜像		
	3、$ sudo service docker stop				//停止 doker 进程
	4、$ docker rm $(docker ps -aq)				//删除所有容器
	5、$ sudo rm /var/lib/docker/network/files/local-kv.db	//删除 local-kv.db 这个文件
	6、$ sudo service docker start				//再启动 docker


/==================================================================================================/
