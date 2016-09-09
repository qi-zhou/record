## 虚拟接迁移
- 配置网卡`/etc/network/interfaces`
```
auto br0
iface br0 inet static
        address 172.16.3.233
        netmask 255.255.0.0
        gateway 172.16.0.1
        dns-nameservers 172.16.0.1
        bridge_ports eth0
        bridge_stp off
        bridge_fd 0
        bridge_maxwait 0
```
启动桥接网卡
sudo ifdown eth0 && sudo ifup br0 && sudo ifup eth0

暂停虚拟机
```
virsh suspend rtdstest2-172.16.250.13
```
迁移虚拟机的配置文件
```
sudo scp sinoiot@172.16.250.2:/etc/libvirt/qemu/rtdstest2-172.16.250.13.xml .
```
迁移虚拟机的镜像
```
rsync -av  sinoiot@172.16.250.2:/home/sinoiot/kvm-rtdstest2-172.16.250.13/tmpYhgUfS.qcow2 .
```
重新定义虚拟机
```
sudo virsh define /etc/libvirt/qemu/rtdstest2-172.16.250.13.xml 
```
启动虚拟机
```
virsh start rtdstest2-172.16.250.13
```

## 安装cm
```
vim /etc/apt/sources.list.d/cloudera.list
# Packages for Cloudera Manager, Version 5, on Ubuntu 14.04 x86_64
deb [arch=amd64] http://cloudera.proxy.ustclug.org/cm5/ubuntu/trusty/amd64/cm trusty-cm5 contrib
deb-src http://cloudera.proxy.ustclug.org/cm5/ubuntu/trusty/amd64/cm trusty-cm5 contrib

wget -qO - http://cloudera.proxy.ustclug.org/cm5/ubuntu/trusty/amd64/cm/archive.key | sudo apt-key add -
sudo apt-get install cloudera-manager-server
```
