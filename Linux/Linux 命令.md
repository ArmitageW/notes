# Linux 命令



## iproute2



Linux 中 iproute2 旨在取代 net-tools，功能强大，能轻松地执行一些网络管理任务。

<img src="Linux 命令.assets/1089507-20170205152617089-1009718040.jpg" alt="1089507-20170205152617089-1009718040"  style="float: left;" />



设置 IP

```
ip addr add 192.168.17.30/24 dev eth0 
```

注意 IP 地址是CIDR（无类别域间路由选择，有时也被称作“超网络”。是一种分派和指定网络地址的方法。）格式 用于显示所用的子网掩码。