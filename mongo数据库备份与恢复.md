
# MongoDB 备份(mongodump)与恢复(mongorestore) #

-	mongoexport/mongoimport导入/导出的是JSON格式，而mongodump/mongorestore导入/导出的是BSON格式。
-	JSON可读性强但体积较大，BSON则是二进制文件，体积小但对人类几乎没有可读性。
-	在一些mongodb版本之间，BSON格式可能会随版本不同而有所不同，所以不同版本之间用mongodump/mongorestore可能
	不会成功，具体要看版本之间的兼容性。当无法使用BSON进行跨版本的数据迁移的时候，使用JSON格式即mongoexport/mongoimport是一个可选项。跨版本的mongodump/mongorestore个人并不推荐，实在要做请先检查文档看两个版本是否兼容（大部分时候是的）。
-	JSON虽然具有较好的跨版本通用性，但其只保留了数据部分，不保留索引，账户等其他基础信息。使用时应该注意。


mongodump命令脚本语法如下：

	>mongodump -h dbhost -d dbname -o dbdirectory
	 -h：
	    MongDB所在服务器地址，例如：127.0.0.1，当然也可以指定端口号：127.0.0.1:27017
	-d：
	    需要备份的数据库实例，例如：test
	-o：
	    备份的数据存放位置，例如：c:\data\dump，当然该目录需要提前建立，在备份完成后，
		系统自动在dump目录下建立一个test目录，这个目录里面存放该数据库实例的备份数据。 

mongodump --host HOST_NAME --port PORT_NUMBER	该命令将备份所有MongoDB数据	
mongodump --host runoob.com --port 27017

mongodump --dbpath DB_PATH --out BACKUP_DIRECTORY		
mongodump --dbpath /data/db/ --out /data/backup/

mongodump --collection COLLECTION --db DB_NAME	该命令将备份指定数据库的集合。	
mongodump --collection mycol --db test

# mongorestore #

mogorestore命令脚本语法如下： 

	>mongorestore -h <hostname><:port> -d dbname <path>

    --host <:port>, -h <:port>：
    MongoDB所在服务器地址，默认为： localhost:27017

    --db , -d ：
    需要恢复的数据库实例，例如：test，当然这个名称也可以和备份时候的不一样，比如test2

    --drop：
    恢复的时候，先删除当前数据，然后恢复备份的数据。就是说，恢复后，备份后添加修改的数据都会被删除，慎用哦！

    path：
    mongorestore 最后的一个参数，设置备份数据所在位置，例如：c:\data\dump\test。

    你不能同时指定 <path> 和 --dir 选项，--dir也可以设置备份目录。
    --dir：

    指定备份的目录

    你不能同时指定 <path> 和 --dir 选项。

# mongoexport #

	参数
		-h:指明数据库宿主机的IP
	
		-u:指明数据库的用户名
		
		-p:指明数据库的密码
		
		-d:指明数据库的名字
		
		-c:指明collection的名字
		
		-f:指明要导出那些列
		
		-o:指明到要导出的文件名
		
		-q:指明导出数据的过滤条件


	[root@localhost mongodb]# ./bin/mongoexport -d test -c students -o students.dat  
	
		-d:指明使用的库，本例中为test
		
		-c:指明要导出的集合，本例中为students
		
		-o:指明要导出的文件名，本例中为students.dat


	./bin/mongoexport -d test -c students --csv -f classid,name,age -o students_csv.dat 

		-csv：指明要导出为csv格式
	
		-f：指明需要导出classid、name、age这3列的数据 

# mongoimport #

	参数
		-h:指明数据库宿主机的IP
	
		-u:指明数据库的用户名
		
		-p:指明数据库的密码
		
		-d:指明数据库的名字
		
		-c:指明collection的名字
		
		-f:指明要导入那些列

	./bin/mongoimport -d test -c students students.dat 

		参数说明：
	
		-d:指明数据库名，本例中为test
		
		-c:指明collection名，本例中为students
		
		students.dat：导入的文件名

	./bin/mongoimport -d test -c students --type csv --headerline --file students_csv.dat   

		-type:指明要导入的文件格式
	
		-headerline:指明第一行是列名，不需要导入
		
		-file：指明要导入的文件




--备份单个表

	mongodump -u  superuser -p 123456  --port 27017 --authenticationDatabase admin -d myTest -c d -o /backup/mongodb/myTest_d_bak_201507021701.bak

--备份单个库

	mongodump  -u  superuser -p 123456 --port 27017  --authenticationDatabase admin -d myTest -o  /backup/mongodb/

--备份所有库

	mongodump  -u  superuser -p 123456 --authenticationDatabase admin  --port 27017 -o /root/bak 

--备份所有库推荐使用添加--oplog参数的命令，这样的备份是基于某一时间点的快照，只能用于备份全部库时才可用，单库和单表不适用：

	mongodump -h 127.0.0.1 --port 27017   --oplog -o  /root/bak 

--同时，恢复时也要加上--oplogReplay参数，具体命令如下(下面是恢复单库的命令)：

	mongorestore  -d swrd --oplogReplay  /home/mongo/swrdbak/swrd/

--恢复单个库：

	mongorestore  -u  superuser -p 123456 --port 27017  --authenticationDatabase admin -d myTest   /backup/mongodb/

--恢复所有库：

	mongorestore   -u  superuser -p 123456 --port 27017  --authenticationDatabase admin  /root/bak

--恢复单表

	mongorestore -u  superuser -p 123456  --authenticationDatabase admin -d myTest -c d /backup/mongodb/myTest_d_bak_201507021701.bak/myTest/d.bson