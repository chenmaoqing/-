# mongo常用操作 #

参考文档：https://www.jb51.net/article/48217.htm

## help ##
	help
	db.help();
	db.yourColl.help();
	db.youColl.find().help();
	rs.help();

## 数据库的操作 ##
	# 查看数据库
		show dbs
	# 切换数据库
		use mydatabase
	# 删除当前数据库
		db.dropDatabase()  #进入需要删除的数据库下执行此命令
	#克隆数据库
		db.cloneDatabase(“127.0.0.1”); 将指定机器上的数据库的数据克隆到当前数据库
		db.copyDatabase("mydb", "temp", "127.0.0.1");将本机的mydb的数据复制到temp数据库中
		db.repairDatabase(); 修复当前数据库
	#获取当前数据库的状态
		db.getName();
		db; 
		db和getName方法是一样的效果，都可以查询当前使用的数据库
		db.stats(); 显示当前db状态
		db.version();  当前db版本
		db.getMongo();  查看当前db的链接机器地址

## 集合操作 Collection聚集集合 ##
	# 查看集合
		show collections
	# 删除集合
		db.users.drop()
	#创建一个聚集集合（table）
		db.createCollection(“collName”, {size: 20, capped: 5, max: 100});//创建成功会显示{“ok”:1}
		//判断集合是否为定容量db.collName.isCapped();
	# 得到指定名称的聚集集合（table）
		db.getCollection("account");
	# 得到当前db的所有聚集集合
		db.getCollectionNames();
	#显示当前db所有聚集索引的状态
		db.printCollectionStats();


## 文档操作 ##

	插入文档
		db.users.insert({
		   name:'harttle',
		   url:'http://harttle.com'
		})
	查询文档
		# 查询所有
			db.users.find()
		# 条件查询
			db.users.find({
			   name:'harttle'
			})
		# 有缩进的输出 db.users.find().pretty()
	更新文档
		db.users.update({
		   name:'harttle'
		}, {
		   url:'http://harttle.com'    
		})
	删除文档
		# 删除所有
			db.users.remove({})
		# 条件删除
			db.users.remove({
			   url:'http://harttle.com'
			})

##用户操作##
	# 添加一个用户
		db.addUser("name");
		db.addUser("userName", "pwd123", true); 添加用户、设置密码、是否只读
	# 数据库认证、安全模式
		db.auth("userName", "123123");
	# 显示当前所有用户
		show users;
	# 删除用户 
		db.removeUser("userName");
	
##查询操作##
	# 去掉查询结果显示id
		
	#查询所有记录
		db.userInfo.find();    # 相当于：select* from userInfo;
	# 查询去掉后的当前聚集集合中的某列的重复数据
		db.userInfo.distinct("name");   会过滤掉name中的相同数据
	# 查询age = 22的记录
		db.userInfo.find({"age": 22});
	# 查询age > 22的记录
		db.userInfo.find({age: {$gt: 22}});
	# 查询age < 22的记录
		db.userInfo.find({age: {$lt: 22}});
	# 查询age >= 25的记录
		db.userInfo.find({age: {$gte: 25}});
	# 查询age <= 25的记录
		db.userInfo.find({age: {$lte: 25}});
	# 查询age >= 23 并且 age <= 26
		db.userInfo.find({age: {$gte: 23, $lte: 26}});
	# 查询name中包含 mongo的数据
		db.userInfo.find({name: /mongo/});
	# 查询name中以mongo开头的
		db.userInfo.find({name: /^mongo/});
	# 查询指定列name、age数据 
		db.userInfo.find({}, {name: 1, age: 1});
	# 查询指定列name、age数据, age > 25
		db.userInfo.find({age: {$gt: 25}}, {name: 1, age: 1});
	# 按照年龄排序
		升序：db.userInfo.find().sort({age: 1});
		降序：db.userInfo.find().sort({age: -1});
	# 查询name = zhangsan, age = 22的数据
		db.userInfo.find({name: 'zhangsan', age: 22});
	# 查询前5条数据
		db.userInfo.find().limit(5);
	# 查询10条以后的数据
		db.userInfo.find().skip(10);
	# 查询在5-10之间的数据
		db.userInfo.find().limit(10).skip(5);
		可用于分页，limit是pageSize，skip是第几页*pageSize
	# or与 查询
		db.userInfo.find({$or: [{age: 22}, {age: 25}]});
	# 查询第一条数据
		db.userInfo.findOne();
	# 查询某个结果集的记录条数
		db.userInfo.find({age: {$gte: 25}}).count();
	# 按照某列进行排序
		db.userInfo.find({sex: {$exists: true}}).count();
	
##索引##
	# 创建索引
		db.userInfo.ensureIndex({name: 1});
		db.userInfo.ensureIndex({name: 1, ts: -1});
	# 查询当前聚集集合所有索引
		db.userInfo.getIndexes();
	# 查看总索引记录大小
		db.userInfo.totalIndexSize();
	# 读取当前集合的所有index信息
		db.users.reIndex();
	# 删除指定索引
		db.users.dropIndex("name_1");	
	# 删除所有索引索引
		db.users.dropIndexes();

##修改、添加、删除集合数据##
	# 添加
		db.users.save({name: ‘zhangsan', age: 25, sex: true});
	# 修改
		db.users.update({age: 25}, {$set: {name: 'changeName'}}, false, true);
		相当于：update users set name = ‘changeName' where age = 25;
		db.users.update({name: 'Lisi'}, {$inc: {age: 50}}, false, true);
		相当于：update users set age = age + 50 where name = ‘Lisi';
		db.users.update({name: 'Lisi'}, {$inc: {age: 50}, $set: {name: 'hoho'}}, false, true);
		相当于：update users set age = age + 50, name = ‘hoho' where name = ‘Lisi';
	# 删除
		db.users.remove({age: 132});
	# 查询修改删除
		db.users.findAndModify({
		    query: {age: {$gte: 25}},    
			#query 查询过滤条件 {} 
		    sort: {age: -1},  
			#如果多个文档符合查询过滤条件，将以该参数指定的排列方式选择出排在首位的对象
		    update: {$set: {name: 'a2'}, $inc: {age: 2}},
		    remove: true    
			#若为true，被选中对象将在返回前被删除 
		});

		db.runCommand({ findandmodify : "users",
		    query: {age: {$gte: 25}},
		    sort: {age: -1},
		    update: {$set: {name: 'a2'}, $inc: {age: 2}},
		    remove: true
		});
	
		#update 或 remove 其中一个是必须的参数; 其他参数可选。

##语句块操作##
	# 简单Hello World
		print("Hello World!");

	# 将一个对象转换成json
		tojson(new Object());
		tojson(new Object('a'));

	# 循环添加数据
		> for (var i = 0; i < 30; i++) {
		... db.users.save({name: "u_" + i, age: 22 + i, sex: i % 2});
		... };
		> for (var i = 0; i < 30; i++) db.users.save({name: "u_" + i, age: 22 + i, sex: i % 2});
		 
	# find 游标查询
		>var cursor = db.users.find();
		> while (cursor.hasNext()) {
		    printjson(cursor.next());
		}

	# forEach迭代循环
		db.users.find().forEach(printjson);
		# forEach中必须传递一个函数来处理每条迭代的数据信息
		db.things.find({x:4}).forEach(function(x) {print(tojson(x));});  # forEach传递函数显示信息


	# 将find游标当数组处理
		var cursor = db.users.find();
		cursor[4];
		# 取得下标索引为4的那条数据
		# 既然可以当做数组处理，那么就可以获得它的长度：cursor.length();或者cursor.count();
		# 那样我们也可以用循环显示数据
		for (var i = 0, len = c.length(); i < len; i++) printjson(c[i]);
	# 将find游标转换成数组
		> var arr = db.users.find().toArray();
		# 用toArray方法将其转换为数组
		> printjson(arr[2]);


	# 定制我们自己的查询结果，只显示age <= 28的并且只显示age这列数据
		db.users.find({age: {$lte: 28}}, {age: 1}).forEach(printjson);
		db.users.find({age: {$lte: 28}}, {age: true}).forEach(printjson);
		db.users.find({age: {$lte: 28}}, {age: false}).forEach(printjson);   #排除age序列

##其他 ##
	# 查询之前的错误信息
		db.getPrevError();
	# 清除错误记录
		db.resetError();

	查看聚集集合基本信息
	1、查看帮助  db.yourColl.help();
	2、查询当前集合的数据条数  db.yourColl.count();
	3、查看数据空间大小 db.userInfo.dataSize();
	4、得到当前聚集集合所在的db db.userInfo.getDB();
	5、得到当前聚集的状态 db.userInfo.stats();
	6、得到聚集集合总大小 db.userInfo.totalSize();
	7、聚集集合储存空间大小 db.userInfo.storageSize();
	8、Shard版本信息  db.userInfo.getShardVersion()
	9、聚集集合重命名 db.userInfo.renameCollection("users"); 将userInfo重命名为users
	10、删除当前聚集集合 db.userInfo.drop();

	show dbs:显示数据库列表
	show collections：显示当前数据库中的集合（类似关系数据库中的表）
	show users：显示用户
	use <db name>：切换当前数据库，这和MS-SQL里面的意思一样
	db.help()：显示数据库操作命令，里面有很多的命令
	db.foo.help()：显示集合操作命令，同样有很多的命令，foo指的是当前数据库下，一个叫foo的集合，并非真正意义上的命令
	db.foo.find()：对于当前数据库中的foo集合进行数据查找（由于没有条件，会列出所有数据）
	db.foo.find( { a : 1 } )：对于当前数据库中的foo集合进行查找，条件是数据中有一个属性叫a，且a的值为1

