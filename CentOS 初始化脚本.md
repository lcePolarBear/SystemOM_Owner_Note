```sh
# 关闭 selinux
setenforce 0
sed -i '/SELINUX/{s/enforcing/disabled/}' /etc/selinux/config
sed -i '/SELINUX/{s/permissive/disabled/}' /etc/selinux/config

# 关闭防火墙
systemctl stop firewalld
systemctl disable firewalld

# 替换 yum 源
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.cloud.tencent.com/repo/centos7_base.repo
curl -o /etc/yum.repos.d/epel.repo http://mirrors.cloud.tencent.com/repo/epel-7.repo
yum clean all
yum makecache

# 重启
reboot
```
