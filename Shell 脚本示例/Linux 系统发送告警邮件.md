## linux 如何发送邮件告警

- 首先安装 mailx 来作为第三方客户端绑定邮箱账号发送邮件
```
yum install mailx
```

- 配置 /etc/mail.rc 文件
```
set from=1198502445@qq.com smtp=smtp.qq.com
set smtp-auth-user=1198502445@qq.com smtp-auth-password='SMTP 授权码'
set smtp-auth=login
```

- 测试能否正常工作
```
echo "this is test mail" | mail -s "monitor" joen.chen@outlook.com
```