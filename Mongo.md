# mongo安装和使用
 #
安装mongo

参考文档：https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-red-hat/

### yum安装，先获取repo源 ###

	[mongodb-enterprise]
	name=MongoDB Enterprise Repository
	baseurl=https://repo.mongodb.com/yum/redhat/$releasever/mongodb-enterprise/3.6/$basearch/
	gpgcheck=1
	enabled=1
	gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc

sudo yum install -y mongodb-enterprise  #安装最新mongo，默认会依赖安装下面的包

	mongodb-enterprise,
	mongodb-enterprise-server,
	mongodb-enterprise-shell,
	mongodb-enterprise-mongos,
	mongodb-enterprise-tools
	
安装指定版本使用下面命令

	yum install -y mongodb-enterprise-3.6.5 mongodb-enterprise-server-3.6.5 mongodb-enterprise-shell-3.6.5 mongodb-enterprise-mongos-3.6.5 mongodb-enterprise-tools-3.6.5

启动使用

	systemctl start mongod
	service mongod start/stop/restart
	chkconfig mongod on
	mongo --host 127.0.0.1:27017

数据目录

	日志：/var/log/mongodb
	数据：/var/lig/mongo
	

### 使用resource安装 ###

获取Mongo的安装包： 

	curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.4.6.tgz
	tar -xf mongodb-linux-x86_64-3.4.6.tgz
	mv mongodb-3.4.6 /usr/local/mongodb
	cd /usr/local/mongodb
	手动创建db和log目录
	mkdir -p /data/db
	mkdir /data/log
	touch /data/log/mongodb.log

启动mongo

	./bin/mongod  --dbpath /data/db --logpath /data/log/mongodb.log --fork --port 27017
	--dbpath 数据存储目录
	--logpath mongo运行日志记录
	--fork 是后台运行
	--port  是运行的端口（默认是27017）
![mongo1](/img/mongo1.jpg)

出现如上图的字样就说明启动成功了


进入数据库

	./bin/mongo  

![mongo2](/img/mongo2.jpg)

### mongodb数据简介 ###
	mongodb是一个介于nosql数据库和mysql数据库之间的一个数据存储系统，它没有严格的数据格式，
	但同时支持复杂查询，而且自带sharding模式和Replica Set模式，支持分片模式，复制模式，
	自动故障处理，自动故障转移，自动扩容，全内容索引，动态查询等功能。扩展性和功能都比较强大。

    mongodb在数据查询方面，支持类sql查询，可以一个key多value内容，可以组合多个value内容来查询，
	支持索引，支持联合索引，支持复杂查询 ，支持排序，基本上除了join和事务类型的操作外，
	mongodb支持所有mysql支持的查询，甚至某个客户端api支持直接使用sql语句查询mongodb。

    mongodb的sharding功能目前日渐完善，支持自定义范围分片，hash自动分片等，分片自动扩容，
	shard之间自动负载均衡等功能。实际使用中功能还不错。

	mongodb 文档数据库,存储的是文档(Bson->json的二进制化).
	特点:内部执行引擎为JS解释器, 把文档存储成bson结构,在查询时,转换为JS对象,并可以通过熟悉的js语法来操作.

	mongo和传统型数据库相比,最大的不同:
	传统型数据库: 结构化数据, 定好了表结构后,每一行的内容,必是符合表结构的,就是说--列的个数,类型都一样.

	mongo文档型数据库: 表下的每篇文档,都可以有自己独特的结构(json对象都可以有自己独特的属性和值)

安装目录下，bin下脚本作用

![mongo3](/img/mongo3.png)

