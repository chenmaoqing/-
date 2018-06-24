# 镜像的save和load #

docker images 命令找到对应需要的images的ID

### docker save 471e783ffeca >cas.tar.gz 将需要的镜像打包 ###

下载和上传到对应所需的服务器

### docker load < cas.tar.gz  将镜像包导入当前的docker程序 ###

	[root@www1 1.0.0428]# docker load < cas.tar.gz 
	1fa8cdcba599: Loading layer [==================================================>]  66.77MB/66.77MB
	a0397ea6bb07: Loading layer [==================================================>]  1.842MB/1.842MB
	33a2bf0294e7: Loading layer [==================================================>]  6.139MB/6.139MB
	ec152eed5632: Loading layer [==================================================>]   5.12kB/5.12kB
	73e8c9461789: Loading layer [==================================================>]  4.608kB/4.608kB
	6027b8431376: Loading layer [==================================================>]  13.58MB/13.58MB
	09c019a81d21: Loading layer [==================================================>]  2.167MB/2.167MB
	e558fb97adf1: Loading layer [==================================================>]  10.24kB/10.24kB
	2ca621711733: Loading layer [==================================================>]  2.158MB/2.158MB
	d5b98082431c: Loading layer [==================================================>]  31.66MB/31.66MB
	16fdc52b14bc: Loading layer [==================================================>]  4.586MB/4.586MB
	4dd56270861b: Loading layer [==================================================>]   38.4kB/38.4kB
	Loaded image ID: sha256:471e783ffecadc1a57b88d8be17930b3411b4f77218ea2716fc8c131d9a12f44
新导入的镜像没有名字和tag，显示none,可以手动加入tag，方便日后识别

	[root@www1 1.0.0428]# docker images
	REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
	192.168.100.10:9000/mdc-user          <none>              fb5798992f66        13 days ago         145MB
	192.168.100.10:9000/mdc-proxy         2.1.0429            516b297cd86d        7 weeks ago         39MB
	<none>                               <none>              471e783ffeca        7 weeks ago         149MB
	192.168.100.10:9000/mongo             <none>              8432ca23076e        8 weeks ago         361MB
	vfarcic/docker-flow-swarm-listener   <none>              d7a2948d9087        7 months ago        21.2MB
	mongo                                <none>              42e262dc0845        8 months ago        361MB
	memcached                            <none>              0447ce4b7d80        11 months ago       7.68MB
	106.2.169.155:5000/umysql            <none>              45377dbce39b        18 months ago       392MB

	[root@www1 0608]# docker tag 471e783ffeca "106.2.169.155:5000/mdc-cas":1.2.1.0.0428 
	[root@www1 1.0.0428]# docker images
	REPOSITORY                           TAG                 IMAGE ID            CREATED             SIZE
	192.168.100.10:9000/mdc-user          <none>              fb5798992f66        13 days ago         145MB
	192.168.100.10:9000/mdc-proxy         2.1.0429            516b297cd86d        7 weeks ago         39MB
	192.168.100.10:9000/mdc-cas           1.2.1.0.0428        471e783ffeca        7 weeks ago         149MB
	192.168.100.10:9000/mongo             <none>              8432ca23076e        8 weeks ago         361MB
	vfarcic/docker-flow-swarm-listener   <none>              d7a2948d9087        7 months ago        21.2MB
	mongo                                <none>              42e262dc0845        8 months ago        361MB
	memcached                            <none>              0447ce4b7d80        11 months ago       7.68MB
	192.168.100.10:9000/umysql            <none>              45377dbce39b        18 months ago       392MB