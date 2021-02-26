### CentOS 设置静态 IP 地址
```yaml
# vi /etc/sysconfig/network-scripts/ifcfg-ens192
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"         # 使用静态IP地址，默认为dhcp
IPADDR="192.168.241.100"   # 设置的静态IP地址
NETMASK="255.255.255.0"    # 子网掩码
GATEWAY="192.168.241.2"    # 网关地址
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="95b614cd-79b0-4755-b08d-99f1cca7271b"
DEVICE="ens33"
ONBOOT="yes"
```

### Ubuntu 设置静态 IP 地址
```yaml
# /etc/netplan/xxx.yaml
network:
    ethernets:
        ens33:
            dhcp4: no
            addresses: [192.168.1.100/24]
            optional: true
            gateway4: 192.168.1.1
            nameservers:
                    addresses: [223.5.5.5,223.6.6.6]
 
    version: 2
```

网关地址和 dns 服务器地址 都可以从 Windows 的网络适配器信息中找到