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
virsh -c qemu+ssh://sinoiot@172.16.250.3/system vncdisplay rtdstest2-172.16.250.13
```
You can tunnel the VNC console connection manually using a standard SSH session if needed (for example, when the virt-viewer utility is unavailable on the management station).

To tunnel the VNC console connection manually, complete the following steps:
Use the virsh command to query the VNC graphical console screen number for the guest you would like to connect to:
# virsh -c qemu+ssh://root@kvmhost.company.org/system vncdisplay guest01
root@kvmhost.company.org's password:
:1
Open an SSH session forwarding the remote VNC port to a local port. To find out which VNC port is being used by your remote KVM guest, add 5900 (the known base VNC port number) to the screen number from the previous step. In this example, the remote VNC port is 5901. The choice of local port is arbitrary. In this example, assume port 5910 (VNC Port for screen number 10) is free in the management station. The following command therefore forwards port 5901 of kvmhost.company.org to port 5910 of the host you are on (management station):
# ssh -L 5910:localhost:5901 root@kvmhost.company.org
    root@kvmhost.company.org's password:
    Last login: Tue Apr 6 15:28:14 2010 from otherclient.company.org
At another terminal, use your VNC client of choice to connect to the local port 5910, where the KVM host's 5901 port is forwarded to. SSH forwards the port and treats the remote port as if it were a local connection attempt:
# vncviewer localhost:10
If this configuration is successful, a VNC session of your remote KVM guest graphical console opens.
```
sudo apt-get install vncviewer

python3 -m http.server 8777
或 python -m SimpleHTTPServer 8080

curl -OL  https://download.teamviewer.com/download/teamviewer_i386.deb
sudo dpkg -i teamviewer_i386.deb
sudo apt-get install -f


## lxd使用
- 安装lxd
```
sudo apt-add-repository ppa:ubuntu-lxc/stable
sudo apt update
sudo apt dist-upgrade
sudo apt install lxd
```
- 安装ZFS，如果是更旧的版本，你需要从 zfsonlinux PPA 安装：
```
sudo apt-add-repository ppa:zfs-native/stable
sudo apt update
sudo apt install ubuntu-zfs
```
 sudo apt-get install lxd-client
sudo lxd init
sudo lxc image list ubuntu:

sudo lxc launch ubuntu:14.04
sudo lxc info adapted-lemming
sudo lxc exec adapted-lemming bash


0是关机
1是单用户

2是多用户，不联网

3是多用户

4是不使用的

5是xwindows，也就是有界面的

6是重启
