*Docker实验性学习和整理*


---

由于Docker Hub 镜像比较慢，所以最好搞个加速地址，我使用的aliyun的
Docker Hub 镜像站点 (加速器地址)。

使用配置文件 /etc/docker/daemon.json（没有时新建该文件）

```shell
{
  "registry-mirrors": ["<your accelerate address>"]
}
```
< your accelerate address > 这个在[ 这里 ][link]注册后，管理后台“Docker Hub 镜像站点”里会提供给你的。
[link]:https://cr.console.aliyun.com/?spm=5176.100239.blogcont29941.13.Grxfgq


### 一些概念
---
docker 可以理解为一种更加轻量的虚拟机，vmwareWorkstation是虚拟多套硬件的话，docker就是虚拟多套系统。

#### 1. 镜像
> 镜像可以push到线上镜像仓库服务商存放，需要时再pull下来使用。

> 镜像 = 基础镜像(一般是系统，如：ubuntu) + 你自己安装在基础镜像里的应用(如：php/mysql/redis等等)

  > 修改镜像：用镜像生成一个容器，在容器里安装你自己的应用，然后提交（docker commit）这个容器生成一个新的镜像（原来的镜像+你安装的应用=新的镜像，可以加上版本号和原来的镜像区别开）。
  如：docker commit -a "runoob.com" -m "my apache" a404c6c174a2  mymysql:v2.0

  > *（docker commit -a "作者" -m "一些信息" <容器ID或容器名称>  <新的镜像名称>:<版本号>）*

#### 2. 容器
  > 容器是通过镜像来创建的，docker可以创建多个容器（成十上百吧，不知道是多少），容器间是独立的隔离的，又是可以相互链接的。同一个镜像可以创建多个不同名的镜像，容器修改后可以commit提交生成新的镜像。





### docker命令的简单解释
  ---
#### 1. 容器相关操作
  ```shell
  Docker create  # 创建一个容器但是不启动它
  Docker run     # 创建并启动一个容器 （推荐使用：docker run -p 80:80 -it --name <容器名> /bin/bash）
  docker stop    # 停止容器运行，发送信号SIGTERM
  Docker start   # 启动一个停止状态的容器
  docker restart # 重启一个容器
  docker rm      # 删除一个容器 （docker rm -f 强制删除容器，即使容器在开启状态）
  docker kill    # 杀掉一个运行中的容器。 (发送信号给容器，默认SIGKILL)
  Docker attach  # 进入一个正在运行容器的命令行模式 (还可以使用：docker exec -it <容器名> /bin/bash)
Docker wait    # 阻塞到一个容器，直到容器停止运行 (不清楚是什么意思)
  Docker exec    # 在容器里执行一个命令（例子：docker exec <容器名> chmod 777 /home/t.txt ）
  ```

#### 2. 获取容器相关信息
  ```shell
  Docker ps      # 显示状态为运行（Up）的容器
docker ps -a   # 显示所有容器,包括运行中（Up）的和退出的(Exited)
  Docker inspect # 深入容器内部获取容器所有信息
docker logs    # 查看容器的日志(stdout/stderr)
  docker events  # 得到docker服务器的实时的事件
  docker port    # 显示容器的端口映射
  Docker top     # 显示容器的进程信息
  Docker diff    # 显示容器文件系统的前后变化
  ```

#### 3. 导出容器
  ```shell
  Docker cp      # 容器与宿主机之间互相拷贝文件或目录
  docker export  # 将容器整个文件系统导出为一个tar包，不带layers、tag等信息
  ```
#### 4. 镜像操作
  ```shell
  docker images  # 显示本地所有的镜像列表
  docker import  # 从一个tar包创建一个镜像，往往和export结合使用
  docker build   # 使用Dockerfile创建镜像（推荐）
  docker commit  # 从容器创建镜像
  docker rmi     # 删除一个镜像
  docker load    # 从一个tar包创建一个镜像，和save配合使用
  docker save    # 将一个镜像保存为一个tar包，带layers和tag信息
  Docker history # 显示生成一个镜像的历史命令
  docker tag     # 为镜像起一个别名
  ```

#### 5. 镜像仓库(registry)操作
  ```shell
  docker login   # 登录到一个registry
  docker search  # 从registry仓库搜索镜像
  Docker pull    # 从仓库下载镜像到本地
  docker push    # 将一个镜像push到registry仓库中

#这些可以在刚才弄镜像加速，也就是阿里云的镜像加速的管理后台创建你自己的镜像仓库后，在“管理”里面有给出命令行的。用用就明白了。
  ```


### 一些问题
  ---

  > 1.在容器里访问外部数据库
  > > 我亲自测试宿主机与容器里是可以互相访问数据库的。前提是MySQL要支持远程访问。
  > > > * 宿主机访问容器里的mysql, 用容器里的IP(ifconfig)。
  > > > * 容器里访问宿主机的mysql, 用宿主机的IP(ifconfig)，还可以用docker0的IP(ifconfig)。
  >
  >  2.容器里的数据库安全
  > > 容器里是可以安装mysql的，但是不建议怎么做，最好让容器里的应用来访问宿主机或远程的mysql。这样数据比较安全。
  >
  > 3.容器里的权限问题
  > > 有时候会遇到在容器里无法写文件，无法使用sudo的问题，等再遇到后在补充。
  >


### 其他
  ---
  > 参考文档
  > > https://blog.csphere.cn/archives/22

  > > http://www.cnblogs.com/52fhy/p/5638571.html

  > > http://www.runoob.com/docker/docker-command-manual.html

  >
  > 阿里云的镜像服务
  > > https://dev.aliyun.com/search.html
  >
