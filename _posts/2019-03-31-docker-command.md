---
layout: post
title:  docker 常用命令
date:   2019-03-31 16:52:00 +0800
categories: document
tag: 教程
---

* content
{:toc}

### docker 命令
#### 用法：`docker [OPTIONS] COMMAND`
#### 选项：
 

    --config string客户端配置文件的位置（默认为“/root/.docker”）
      -D， - debug启用调试模式
      -H， - 主机列表要连接的守护程序套接字
      -l， - log-level string设置日志记录级别（“debug”|“info”|“warn”|“error”|“fatal”）（默认为“info”）
          --tls使用TLS;由--tlsverify暗示
          --tlscacert字符串仅由此CA签名的信任证书（默认为“/root/.docker/ca.pem”）
          --tlscert string TLS证书文件的路径（默认为“/root/.docker/cert.pem”）
          --tlskey string TLS密钥文件的路径（默认为“/root/.docker/key.pem”）
          --tlsverify使用TLS并验证远程
      -v， - version打印版本信息并退出
#### 管理命令：

    builder     管理构建
    config      管理Docker配置
    container   管理容器
    engine      管理docker引擎
    image       管理镜像
    network     管理网络
    node        管理Swarm节点
    plugin      管理插件
    secret      管理Docker secrets
    service     管理服务
    stack       管理Docker堆栈
    swarm       管理Swarm
    system     	管理Docker
    trust       管理对Docker镜像的信任
    volume      管理卷
#### 命令：
  

      attach	将本地标准输入，输出和错误流附加到正在运行的容器
      build		从Dockerfile构建映像
      commit	从容器的更改创建新图像
      cp		在容器和本地文件系统之间复制文件/文件夹
      create	创建一个新容器
      diff		检查容器文件系统上的文件或目录的更改
      events	从服务器获取实时事件
      exec		在正在运行的容器中运行命令
      export	将容器的文件系统导出为tar存档
      history	显示图像的历史记录
      images	列出图像
      import	从tarball导入内容以创建文件系统映像
      info		显示系统范围的信息
      inspect	返回Docker对象的低级信息
      kill		杀死一个或多个正在运行的容器
      load		从tar存档或STDIN加载图像
      login		登录Docker注册表
      logout	从Docker注册表注销
      logs		获取容器的日志
      pause		暂停一个或多个容器中的所有进程
      port		列出端口映射或容器的特定映射
      ps		列出容器
      pull		从注册表中提取图像或存储库
      push		将映像或存储库推送到注册表
      rename	重命名容器
      restart	重新启动一个或多个容器
      rm		移除一个或多个容器
      rmi		删除一张或多张图像
      run		在新容器中运行命令
      save		将一个或多个图像保存到tar存档（默认情况下流式传输到STDOUT）
      search	在Docker Hub中搜索图像
      start		启动一个或多个已停止的容器
      stats		显示容器资源使用情况统计信息的实时流
      stop		停止一个或多个正在运行的容器
      tag		创建引用SOURCE_IMAGE的标记TARGET_IMAGE
      top		显示容器的运行进程
      unause	取消暂停一个或多个容器中的所有进程
      update	更新一个或多个容器的配置
      version	显示Docker版本信息
      wait		阻止直到一个或多个容器停止，然后打印退出代码

###  docker run 命令
#### 用法：`docker run [OPTIONS] IMAGE [COMMAND] [ARG ...]`
#### 选项：

          --add-host list添加自定义主机到IP映射（host：ip）
      -a，--attach list附加到STDIN，STDOUT或STDERR
          --blkio-weight uint16块IO（相对权重），介于10和1000之间，或0表示禁用（默认为0）
          --blkio-weight-device list阻塞IO权重（相对设备权重）（默认为[]）
          --cap-add list添加Linux功能
          --cap-drop list删除Linux功能
          --cgroup-parent string容器的可选父cgroup
          --cidfile string将容器ID写入文件
          --cpu-period int限制CPU CFS（完全公平调度程序）期间
          --cpu-quota int限制CPU CFS（完全公平调度程序）配额
          --cpu-rt-period int限制CPU实时周期（以微秒为单位）
          --cpu-rt-runtime int以微秒为单位限制CPU实时运行时间
      -c， - cpu-shares int CPU份额（相对权重）
          --cpus decimal CPU数
          --cpuset-cpus字符串允许执行的CPU（0-3,0,1）
          --cpuset-mems string允许执行的MEMs（0-3,0,1）
      -d， - detach在后台运行容器并打印容器ID
          --detach-keys string重写用于分离容器的键序列
           - 设备列表将主机设备添加到容器中
          --device-cgroup-rule list将规则添加到cgroup允许的设备列表中
          --device-read-bps list限制设备的读取速率（每秒字节数）（默认为[]）
          --device-read-iops list从设备限制读取速率（每秒IO）（默认为[]）
          --device-write-bps list限制设备的写入速率（每秒字节数）（默认为[]）
          --device-write-iops list限制设备的写入速率（每秒IO）（默认为[]）
          --disable-content-trust跳过图像验证（默认为true）
          --dns list设置自定义DNS服务器
          --dns-option list设置DNS选项
          --dns-search list设置自定义DNS搜索域
          --entrypoint string覆盖图像的默认ENTRYPOINT
      -e， - env list设置环境变量
          --env-file list读入环境变量文件
          --expose list公开端口或端口范围
          --group-add list添加要加入的其他组
          --health-cmd string运行以检查运行状况的命令
          --health-interval duration运行检查之间的时间（ms | s | m | h）（默认为0s）
          --health-retries int报告不健康所需的连续失败
          --health-start-period duration开始健康重试倒计时之前容器初始化的开始时间（ms | s | m | h）（默认为0）
          --health-timeout duration允许一次检查运行的最长时间（ms | s | m | h）（默认为0s）
          --help打印用法
      -h， - hostname string容器主机名
          --init在容器内运行init，转发信号并重新获得进程
      -i， - interactive尽管未连接，仍保持STDIN打开
          --ip字符串IPv4地址（例如，172.30.100.104）
          --ip6字符串IPv6地址（例如，2001：db8 :: 33）
          --ipc string IPC模式使用
           - 隔离字符串容器隔离技术
          --kernel-memory bytes内核内存限制
      -l， - label列表在容器上设置元数据
          --label-file list读入行分隔的标签文件
          --link list添加到另一个容器的链接
          --link-local-ip list容器IPv4 / IPv6链路本地地址
          --log-driver string容器的日志记录驱动程序
          --log-opt list日志驱动程序选项
          --mac-address string容器MAC地址（例如，92：d0：c6：0a：29：33）
      -m， - memory bytes内存限制
          --memory-reservation bytes内存软限制
          --memory-swap bytes交换限制等于内存加交换：' - 1表示启用无限制交换
          --memory-swappiness int调整容器内存swappiness（0到100）（默认-1）
          --mount mount将文件系统挂载附加到容器
          --name string为容器指定名称
          --network string将容器连接到网络（默认为“default”）
          --network-alias list为容器添加网络范围的别名
          --no-healthcheck禁用任何容器指定的HEALTHCHECK
          --oom-kill-disable禁用OOM杀手
          --oom-score-adj int调整主机的OOM首选项（-1000到1000）
          --pid字符串PID命名空间使用
          --pids-limit int调整容器pids限制（设置-1表示无限制）
          --privileged为此容器提供扩展权限
      -p， - 发布列表将容器的端口发布到主机
      -P， - publish-all将所有公开的端口发布到随机端口
          --read-only将容器的根文件系统挂载为只读
          --restart string重新启动容器退出时应用的策略（默认为“no”）
          --rm退出时自动删除容器
          --runtime string用于此容器的运行时
          --security-opt列表安全选项
          --shm-size bytes / dev / shm的大小
          --sig-proxy Proxy接收到进程的信号（默认为true）
          --stop-signal string停止容器的信号（默认为“SIGTERM”）
          --stop-timeout int停止容器的超时（以秒为单位）
          --storage-opt list容器的存储驱动程序选项
          --sysctl map Sysctl选项（默认map []）
          --tmpfs list挂载一个tmpfs目录
      -t， - try分配伪TTY
          --ulimit ulimit Ulimit选项（默认[]）
      -u， - user string用户名或UID（格式：<name | uid> [：<group | gid>]）
          --userns string要使用的用户名空间
           - 要使用的字符串UTS名称空间
      -v， - volume list绑定装入卷
          --volume-driver string容器的可选卷驱动程序
          --volumes-from list从指定容器装载卷
      -w， - workdir string容器内的工作目录

