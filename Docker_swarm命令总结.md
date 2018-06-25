Docker swarm

初始化swarm manager并制定网卡地址

```
docker swarm init --advertise-addr 192.168.10.1171
```

强制删除集群，如果是manager，需要加–force

```
docker swarm leave --force
docker node rm docker-11812
```

查看swarm worker的连接令牌

```
docker swarm join-token worker1
```

查看swarm manager的连接令牌

```
docker swarm join-token manager1
```

使旧令牌无效并生成新令牌

```
docker swarm join-token --rotate1
```

加入docker swarm集群

```
docker swarm join --token SWMTKN-1-5d2ipwo8jqdsiesv6ixze20w2toclys76gyu4zdoiaf038voxj-8sbxe79rx5qt14ol14gxxa3wf 192.168.10.117:23771
```

查看集群中的节点

```
docker node ls
```

查看集群中节点信息

```
docker node inspect docker-117 --pretty1
```

调度程序可以将任务分配给节点

```
docker node update --availability active docker-1181
```

调度程序不向节点分配新任务，但是现有任务仍然保持运行

```
docker node update --availability pause docker-1181
```

调度程序不会将新任务分配给节点。调度程序关闭任何现有任务并在可用节点上安排它们

```
docker node update --availability drain docker-1181
```

添加节点标签

```
docker node update --label-add label1 --label-add bar=label2 docker-1171
```

删除节点标签

```
docker node update --label-rm label1 docker-1171
```

将节点升级为manager

```
docker node promote docker-1181
```

将节点降级为worker

```
docker node demote docker-1181
```

查看服务列表

```
docker service ls1
```

查看服务的具体信息

```
docker service ps redis1
```

创建一个不定义name，不定义replicas的服务

```
docker service create nginx1
```

创建一个指定name的服务

```
docker service create --name my_web nginx1
```

创建一个指定name、run cmd的服务

```
docker service create --name helloworld alping ping docker.com1
```

创建一个指定name、version、run cmd的服务

```
docker service create --name helloworld alping:3.6 ping docker.com1
```

创建一个指定name、port、replicas的服务

```
docker service create --name my_web --replicas 3 -p 80:80 nginx1
```

为指定的服务更新一个端口

```
docker service update --publish-add 80:80 my_web1
```

为指定的服务删除一个端口

```
docker service update --publish-rm 80:80 my_web1
```

将redis:3.0.6更新至redis:3.0.7

```
docker service update --image redis:3.0.7 redis1
```

配置运行环境，指定工作目录及环境变量

```
docker service create --name helloworld --env MYVAR=myvalue --workdir /tmp --user my_user alping ping docker.com1
```

创建一个helloworld的服务

```
docker service create --name helloworld alpine ping docker.com1
```

更新helloworld服务的运行命令

```
docker service update --args “ping www.baidu.com” helloworld1
```

删除一个服务

```
docker service rm my_web1
```

在每个群组节点上运行web服务

```
docker service create --name tomcat --mode global --publish mode=host,target=8080,published=8080 tomcat:latest1
```

创建一个overlay网络

```
docker network create --driver overlay my_network
docker network create --driver overlay --subnet 10.10.10.0/24 --gateway 10.10.10.1 my-network12
```

创建服务并将网络添加至该服务

```
docker service create --name test --replicas 3 --network my-network redis1
```

删除群组网络

```
docker service update --network-rm my-network test1
```

更新群组网络

```
docker service update --network-add my_network test1
```

创建群组并配置cpu和内存

```
docker service create --name my_nginx --reserve-cpu 2 --reserve-memory 512m --replicas 3 nginx1
```

更改所分配的cpu和内存

```
docker service update --reserve-cpu 1 --reserve-memory 256m my_nginx1
```

指定每次更新的容器数量

```
--update-parallelism1
```

指定容器更新的间隔

```
--update-delay1
```

定义容器启动后监控失败的持续时间

```
--update-monitor 1
```

定义容器失败的百分比

```
--update-max-failure-ratio1
```

定义容器启动失败之后所执行的动作

```
--update-failure-action1
```

创建一个服务并运行3个副本，同步延迟10秒，10%任务失败则暂停

```
docker service create --name mysql_5_6_36 --replicas 3 --update-delay 10s --update-parallelism 1 --update-monitor 30s --update-failure-action pause --update-max-failure-ratio 0.1 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.6.361
```

回滚至之前版本

```
docker service update --rollback mysql1
```

自动回滚 
 如果服务部署失败，则每次回滚2个任务，监控20秒，回滚可接受失败率20%

```
docker service create --name redis --replicas 6 --rollback-parallelism 2 --rollback-monitor 20s --rollback-max-failure-ratio .2 redis:latest1
```

创建服务并将目录挂在至container中

```
docker service create --name mysql --publish 3306:3306 --mount type=bind,src=/data/mysql,dst=/var/lib/mysql --replicas 3 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.6.361
```

Bind带来的风险 
 1、绑定的主机路径必须存在于每个集群节点上，否则会有问题 
 2、调度程序可能会在任何时候重新安排运行服务容器，如果目标节点主机变得不健康或无法访问 
 3、主机绑定数据不可移植，当你绑定安装时，不能保证你的应用程序开发方式与生产中的运行方式相同

添加swarm配置

```
echo "this is a mysql config" | docker config create mysql -1
```

查看配置

```
docker config ls1
```

查看配置详细信息

```
docker config inspect mysql1
```

删除配置

```
docker config rm mysql1
```

添加配置

```
docker service update --config-add mysql mysql1
```

删除配置

```
docker service update --config-rm mysql mysql1
```

添加配置

```
docker config create homepage index.html1
```

启动容器的同时添加配置

```
docker service create --name nginx --publish 80:80 --replicas 3 --config src=homepage,target=/usr/share/nginx/html/index.html nginx 
```