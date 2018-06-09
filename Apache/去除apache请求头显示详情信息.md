# 去除apache请求头显示详情信息

httpd.conf 打开注释

```
# Various default settings
Include conf/extra/httpd-default.conf
```

extra/httpd-default.conf

```
ServerTokens Full | OS | Minor | Minimal | Major | Prod
ServerSignature On | Off | EMail
```

php.ini

```
expose_php = On
```

