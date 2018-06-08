### my.ini
```
[client]
port=3306
default-character-set = utf8
[mysqld]
port=3306
basedir=F:\applib\wamp\mysql-5.7.18
datadir=F:\applib\wamp\mysql-5.7.18\data
character_set_server = utf8
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
```
针对于windows下面的mysql5.7的配置

### 安装配置

>https://blog.csdn.net/haixwang/article/details/54973036

### 初始化
```
mysqld --initialize-insecure自动生成无密码的root用户，mysqld --initialize自动生成带随机密码的root用户
```
### 设置密码

```
net stop mysql    回车    
进入到mysql\bin\ 目录下，执行mysqld --skip-grant-tables
UPDATE user SET authentication_string = PASSWORD('Admin_888') WHERE user = 'root';
FLUSH PRIVILEGES;
```

退出，重启服务，登录，大功告成。
