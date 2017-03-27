#编写mysql插件

##查看mysql插件so目录

###进入mysql后执行如下指令

	SHOW VARIABLES LIKE 'plugin_dir';

显示

	plugin_dir =/usr/local/mysql/lib/plugin/
编译

	gcc $(mysql_config --cflags) -shared -fPIC -o calc_distance_udf.so calc_distance_udf.c

将生成so拷到指定目录

	cp calc_distance_udf.so /usr/local/mysql/lib/plugin/

进入mysql创建funcation

	CREATE FUNCTION calc_distance_udf 
	   RETURNS REAL
	   SONAME "calc_distance_udf.so";
##注意
	如果以上创建function出错的时候要检查下so文件的权限是否可读。
查看funcation
	USE mysql;
	SELECT * FROM func;

测试

 	select calc_distance_udf(1.0, 2.0, 3.0, 4.0);

参考教程

[http://blog.loftdigital.com/blog/how-to-write-mysql-functions-in-c](http://blog.loftdigital.com/blog/how-to-write-mysql-functions-in-c)

#注意：
 `mysql udf`的执行方式是mysql程序启动的时候加载so文件到内存，有`FUNCATION`调用的时候，执行so里面的程序。所以更新so文件后要重启mysql服务。

如果使用Ubuntu的apt直接安装mysql，编译的时候会出现找不到相应的头文件，执行以下命令

	sudo apt-get install libmysql++-dev

还有在不同的平台下要使用对应的编译器重新编译，32位对应32位，64位对应64位。