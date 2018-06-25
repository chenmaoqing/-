# 下载项目
	#拉取远程项目，将本地ssh-key放入远程GitHub的ssh秘钥管理
	然后通过下面的命令拉取远程项目，以后再进行代码提交的时候就可以免密码提交代码了
	git init
	git clone  ssh://git@github.com/chenmaoqing/chenmaoqing.git
	cd chenmaoqing
	git pull    #就可以将远程最新代码拉取到本地
	
# git 上传更新代码步骤
	- git pull   #先拉取最新代码
	- 将自己的写好的代码放入到相应的文件夹下，比如op/issues
	- 如果有图片将对应的图片也复制到当前目录下的img下
	- git add 自己加入到的文件和图片
	- git commit 自己加入和文件和图片 -m  message
	- 这里如果不加-m message 选项可能会push不成功，最好加上
	- git push   #上传自己新加的文件到GitHub
	 将本地远程项目拉取到本地
	
	
# markdown语法总结
## - 图片
	方法一：
		- 加入图片，格式是 ![]()
		- 其中的[]中的内容可以自定义，比如[log]
		- ()中写使用该图片的文件相对图片的相对路径
	方法二：
		<img src="/img/1.jpg" width=256 height=256 />

###  图片引用方法一实例
	此时img文件夹和当前文档在同一级目录下
![first_img](/img/1.jpg)
![first_img](/img/2.jpg)
###  图片引用方法二实例，可以控制图片大小
<img src="/img/1.jpg" width=256 height=256 />
<img src="/img/2.jpg" width=256 height=256 />
### 让图片居中的方法，使用div标签
<div align=center>
<img src="/img/1.jpg" width=256 height=256 />
</div>
	
## - 斜体
	- 用左右各一个 *的方式包裹住文字就是斜体的语法
## - 粗体
	- 用左右各两个**的方式包裹住文字就是粗体的语法
## - 改变字体大小
	- 一个字、一句话、一个段落加上#就可以改变字体的大小
## - 分割线
	- 输入三个--就可以得到分割线
## - 引用
	-在内容首位加入>符号即可
## - 表格
	1、 原生的表格语法
	 
		| 嘻嘻 | 哈哈 | 呵呵
		| :------------- :|:-------------:| :-----:|
		| 你好|我好|大家好 |
		| 是的| 是的 | 是的 |
    
	2、 也可以使用html语言来实现，实例如下
	
		<table>
		  <tr>
		    <th width=10%, bgcolor=yellow >参数</th>
		    <th width=40%, bgcolor=yellow>详细解释</th>
		    <th width="50%", bgcolor=yellow>备注</th>
		  </tr>
		  <tr>
		    <td bgcolor=#eeeeee> -l </td>
		    <td> use a long listing format  </td>
		    <td> 以长列表方式显示（显示出文件/文件夹详细信息）  </td>
		  </tr>
		  <tr>
		    <td bgcolor=#00FF00>-t </td>
		    <td> sort by modification time </td>
		    <td> 按照修改时间排序（默认最近被修改的文件/文件夹排在最前面） </td>
		  <tr>
		    <td bgcolor=rgb(0,10,0)>-r </td>
		    <td> reverse order while sorting </td>
		    <td>  逆序排列 </td>
		  </tr>
		</table>
		
# html方式实现表格实例

<table>
  <tr>
    <th width=10%, bgcolor=yellow >参数</th>
    <th width=40%, bgcolor=yellow>详细解释</th>
    <th width="50%", bgcolor=yellow>备注</th>
  </tr>
  <tr>
    <td bgcolor=#eeeeee> -l </td>
    <td> use a long listing format  </td>
    <td> 以长列表方式显示（显示出文件/文件夹详细信息）  </td>
  </tr>
  <tr>
    <td bgcolor=#00FF00>-t </td>
    <td> sort by modification time </td>
    <td> 按照修改时间排序（默认最近被修改的文件/文件夹排在最前面） </td>
  <tr>
    <td bgcolor=rgb(0,10,0)>-r </td>
    <td> reverse order while sorting </td>
    <td>  逆序排列 </td>
  </tr>
</table>
