## Linux 系统配置初始化

- 设置时区并同步时间
    ```
    ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    ```
    - 将系统时区软连接为目标地区时区
    ```
    if ! crontab -l | grep ntpdate &>/dev/null ; then
        (echo "* 1 * * * ntpdate time.windows.com >/dev/null 2>&1";crontab -l) |crontab 
    fi
    ```
    - 使用 if 判断任务计划中是否有时间同步计划，如果没有则添加
        - 使用 `ntpdate time.windows.com` 命令能够实现时间同步
        - 使用管道将内容写入时是覆盖模式而非追加，故要使用 `crontab -l` 写入原来的任务计划
        - 可以用 `echo $?` 显示上一个命令的 true | false

- 禁用 selinux
    ```
    sed -i '/SELINUX/{s/permissive/disabled/}' /etc/selinux/config
    ```
    - 将 config 文件下 SELINUX 关键字所在行的第一个 permissive 内容替换为 disabled

- 关闭防火墙
    ```
    if egrep "7.[0-9]" /etc/redhat-release &>/dev/null; then
        systemctl stop firewalld
        systemctl disable firewalld
    elif egrep "6.[0-9]" /etc/redhat-release &>/dev/null; then
        service iptables stop
        chkconfig iptables off
    fi
    ```
    - 判断 7.[0-9] 的内容是否在 redhat-release 内容下

- 历史命令显示操作时间
    ```
    if ! grep HISTTIMEFORMAT /etc/bashrc; then
        echo 'export HISTTIMEFORMAT="%F %T `whoami` "' >> /etc/bashrc
    fi
    ```
    - `history` 命令用来显示历史命令，通过引入 HISTTIMEFORMAT 变量实现显示操作时间
    
- SSH 超时时间
    ```
    if ! grep "TMOUT=600" /etc/profile &>/dev/null; then
        echo "export TMOUT=600" >> /etc/profile
    fi
    ```

- 禁止 root 远程登录(慎用！执行前确认可以通过别的 root 权限账户可以登录)
    ```
    sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
    ```

- 禁止定时任务发送邮件
    ```
    sed -i 's/^MAILTO=root/MAILTO=""/' /etc/crontab 
    ```
    - /var/mail/ 目录下存放发送的邮件
    - `&>/dev/null` 指令就是为了阻止产生日志文件而避免产生邮件
    - 将以 ^MAILTO=root 开头的这一行内容替换为 MAILTO=""

- 设置最大打开文件数
    ```
    if ! grep "* soft nofile 65535" /etc/security/limits.conf &>/dev/null; then
    cat >> /etc/security/limits.conf << EOF
    *   soft    nofile  65535
    *   hard    nofile  65535
    EOF
    fi
    ```

- 系统内核优化
    ```
    cat >> /etc/sysctl.conf << EOF
    net.ipv4.tcp_syncookies = 1
    net.ipv4.tcp_max_tw_buckets = 20480
    net.ipv4.tcp_max_syn_backlog = 20480
    net.core.netdev_max_backlog = 262144
    net.ipv4.tcp_fin_timeout = 20  
    EOF
    ```
- 减少 SWAP 使用
    ```
    echo "0" > /proc/sys/vm/swappiness
    ```
- 安装系统性能分析工具及其他
    ```
    yum install gcc make autoconf vim sysstat net-tools iostat iftop iotp lrzsz -y
    ```