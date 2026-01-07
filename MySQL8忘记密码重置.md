Linux下MySQL8 忘记密码重置

1、 修改MySQL的配置文件（默认为/etc/my.cnf，可先查看配置文件构造）,在[mysqld]下添加一行

	[mysqld]
	
	skip-grant-tables

2、 保存配置文件后重启mysql  service mysql restart

3、 mysql -uroot -p，输入密码是直接回车，进入MySQL数据库

4、 改密码 ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123';

	若第四步报错 ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option，先执行flush privileges; 再执行改密码操作

5、 将配置文件的skip-grant-tables去掉，重启mysql

6、 用mysql -uroot -p 输入密码登录