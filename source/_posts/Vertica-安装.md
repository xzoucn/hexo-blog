---
title: Vertica 安装
date: 2019-03-15 14:16:18
tags: vertica
categories: 数据库
---
# 标准安装
## 原料
- CentOS Linux release 7.5.1804
- vertica-9.2.0-0.x86_64.RHEL6.rpm

## 安装
切换到root权限 
``` shell
su
```
安装dialog依赖
``` shell
yum install dialog -y 
```
安装下载的rpm安装包
``` shell
rpm -ivh vertica-9.2.0-0.x86_64.RHEL6.rpm
```
上一步完成后会提示需要通过install_vertica程序来继续完成安装
``` shell
Vertica Analytic Database V9.2.0-0 successfully installed on host host1

To complete your NEW installation and configure the cluster, run: 
 /opt/vertica/sbin/install_vertica

To complete your Vertica UPGRADE, run:
 /opt/vertica/sbin/update_vertica

---------------------------------------------------------------------------------- 
Important
---------------------------------------------------------------------------------- 
Before upgrading Vertica, you must backup your database.  After you restart your   
database after upgrading, you cannot revert to a previous Vertica software version.
---------------------------------------------------------------------------------- 

View the latest Vertica documentation at http://my.vertica.com/docs/
```
install_vertica的用法
``` shell
用法: 
  # 安装或更新:
  install_vertica --hosts host1,host2,host3 --rpm vertica.rpm
  install_vertica --hosts 192.168.1.101,192.168.1.101,192.168.1.102 \
          --rpm vertica.rpm

  # 添加或删除节点
  install_vertica --add-hosts host4 --rpm vertica.rpm
  install_vertica --remove-hosts host4

  # 获取帮助
  install_vertica --help
```
完成安装配置（待续）
``` shell
install_vertica --hosts host1,host2,host3 --rpm vertica.rpm
```

# Docker安装
标准安装对于软硬件有诸多依赖，通过Docker镜像安装则提供了一种开箱即用的安装方式。下面我们将一步步来通过Docker安装Vertica

安装yum-utils、device-mapper-persistent-data、lvm2
``` shell
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
配置配置docker yum repo
``` shell
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
通过yum安装docker-ce
``` shell
sudo yum install docker-ce
```
启动docker服务
``` shell
sudo systemctl start docker
```
 拉取docker镜像
```
docker pull jbfavre/vertica:8.1.1-11_debian-8
```
运行docker镜像，映射内外端口、持久化存储
``` shell
docker run -p 5433:5433 -d \
           -v /data01/docker-vertica/data:/home/dbadmin/docker \
           jbfavre/vertica:8.1.1-11_debian-8
```
获取vsql客户端
``` shell
wget https://www.vertica.com/client_drivers/8.1.x/8.1.1-13/vertica-client-8.1.1-13.x86_64.rpm
```
安装vsql客户端
``` shell
rpm -ivh vertica-client-8.1.1-13.x86_64.rpm
```
运行客户端
``` shell
/opt/vertica/bin/vsql -U dbadmin
```








