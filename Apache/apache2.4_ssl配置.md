# 生成证书

## 安装git

```
# ubuntu
apt-get -y install git
# centos
yum -y install git-core
```

## 下载和生成证书

```
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt
./certbot-auto certonly --standalone --email admin@admin.com -d www.demo.cn

# 看到这个界面，直接Agree回车。

```

## 查看生成的证书

```
ls /etc/letsencrypt/live/ www.demo.cn/

cert.pem  - 服务器端证书
chain.pem  - 根证书和中继证书
fullchain.pem  - 有公钥和私钥的文件建议用它
privkey.pem - 安全证书KEY文件
```



## **解决Let's Encrypt免费SSL证书有效期问题** 

```
./letsencrypt-auto certonly --renew-by-default --email admin@admin.com -d www.demo.com
```

这样我们在90天内再去执行一次就可以解决续期问题，这样又可以继续使用90天。如果我们怕忘记的话也可以制作成定时执行任务，比如每个月执行一次。



# 虚拟主机

```
<VirtualHost *:80>
        ServerName www.demo.com
        ErrorLog /website/logs/error.log
        CustomLog /website/logs/access.log combined
        RewriteEngine on
        RewriteRule ^(.*)$ https://wx.demo.cn$1 [R=301,L]
</VirtualHost>

<VirtualHost *:443>
        DocumentRoot /website
        ServerName www.demo.com
        ErrorLog /website/logs/error.log
        CustomLog /website/logs/access.log combined

        SSLEngine on
        SSLCipherSuite ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4:!DH:!DHE
        SSLProtocol TLSv1.1 TLSv1.2
        SSLHonorCipherOrder on
        SSLCertificateFile /etc/letsencrypt/live/wx.1314000.cn/fullchain.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/wx.1314000.cn/privkey.pem
        Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        <Directory "/website">
            # 指定目录启用特怀
            #Options Indexes FollowSymLinks 
            Options FollowSymLinks
            # 是否允许使用.htaccess文件
            AllowOverride All
            # 访问目录权限
            Require all granted
        </Directory>
</VirtualHost>

```

# 安装出现的问题

certbot脚本基于python2，当系统里有python2 和python3时，会报错： 

```
OSError: Command /root/.local/share/letsencrypt/bin/python2.7 - setuptools pkg_resources pip wheel failed with error code 2
Let's Encrypt returned an error status. Aborting.
```

解决办法：重新安装virtualenv环境（有效）：  

先卸载：

```
apt-get purge python-virtualenv python3-virtualenv virtualenv
```
再安装：

```
pip install virtualenv1
```

注意，安装在python2环境下，运行certbot命令后又会安装virtualenv环境



切换python2/3 

https://blog.csdn.net/setoy/article/details/78587723