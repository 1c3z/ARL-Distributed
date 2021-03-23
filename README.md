## 简介

ARL Docker 环境分布式部署配置文件


# ARL 分布式部署详细说明

## 架构图

用户通过登录后台界面，将任务信息提交给Master，Master通过调度将任务随机下发到下面的3个Worker.

![image](https://user-images.githubusercontent.com/9052484/112109799-f8f0b780-8bec-11eb-9688-f90d372151e2.png)


## 克隆配置文件

在Master和不同的Worker上执行
```
git clone  https://github.com/1c3z/ARL-Distributed
```

实际使用请修改`master/docker-compose.yml` 中配置的`mongo` 和`rabbitmq`密码。
并同步修改`config-docker.yaml` 中的`mongo`和 `rabbitmq` 密码
以及将`arl-master` 修改为 `Master` 对应的公网IP, 并允许能通过公网访问到`5003`，`27017`、`5672` 端口

## 部署 Master
为了部署简单，我们将后台Web系统, mongo, 以及rabbitmq 和 scheduler 都部署到Master上。

启动并观察是否生效
```
cd ARL-Distributed/master
docker-compose up -d
docker-compose ps
```

## 部署 Worker
并同步修改 `worker/config-docker.yaml` 中的mongo和 rabbitmq 密码
以及将`arl-master`修改为`Master`对应的公网IP, 并确保Worker能访问到Master 的 `5003`，`27017`、`5672` 端口

可以根据自己的需求修改`worker/docker-compose.yml` entrypoint 中的 `-c 2` 参数，默认是2个并发，并发数最好少于等于CPU核数。

在不同的Worker上启动并观察是否生效
```
cd ARL-Distributed/worker
docker-compose up -d
docker-compose ps
```

第一台Worker  
![image](https://user-images.githubusercontent.com/9052484/112109840-09a12d80-8bed-11eb-8c8f-f66c63559819.png)



第二台Worker   
![image](https://user-images.githubusercontent.com/9052484/112109890-158cef80-8bed-11eb-97f8-e4cc7bff753c.png)


## 任务下发

通过  https://vps:5003/  登录下发多个任务，并观察到`arl_worker.log`都接收到了任务。

## 已知缺点

1. 无法控制下发的任务分发到哪一个Worker
2. 网站截图图片在后台系统查看不到。
3. 密码配置等信息要写多处。（逃



## 链接
https://github.com/TophantTechnology/ARL