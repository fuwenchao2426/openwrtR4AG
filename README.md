# 小米路由器 R4A 千兆版 刷Openwrt 官方固件 完整教程 

[TOC]

## 0.准备工作&说明

硬件: 小米路由器4A千兆版 20.28.XX 固件版本适用





## 1.开启SSH权限

```shell
python remote_command_execution_vulnerability.py
```



## 2.备份



```shell
cd /tmp
mkdir backup
cd backup
dd if=/dev/mtd0 of=/tmp/backup/ALL.bin
dd if=/dev/mtd1 of=/tmp/backup/Bootloader.bin
dd if=/dev/mtd2 of=/tmp/backup/Config.bin
dd if=/dev/mtd3 of=/tmp/backup/Bdata.bin
dd if=/dev/mtd4 of=/tmp/backup/Factory.bin
dd if=/dev/mtd5 of=/tmp/backup/crash.bin
dd if=/dev/mtd6 of=/tmp/backup/cfg_bak.bin
dd if=/dev/mtd7 of=/tmp/backup/overlay.bin
dd if=/dev/mtd8 of=/tmp/backup/OS1.bin
dd if=/dev/mtd9 of=/tmp/backup/rootfs.bin
dd if=/dev/mtd10 of=/tmp/backup/disk.bin

```

[进入Breed](## 进入Breed) 

## 3.刷入Breed

```shell
mtd -r write /tmp/breed-mt7621-pbr-m1.bin Bootloader
```



## 4.刷入Openwrt官方固件

[下载地址](https://downloads.openwrt.org/)

[最新稳定版](https://downloads.openwrt.org/releases/23.05.4/targets/ramips/mt7621/openwrt-23.05.4-ramips-mt7621-xiaomi_mi-router-4a-gigabit-squashfs-sysupgrade.bin)

```shell
wget http://192.168.1.2/openwrt-23.05.4-ramips-mt7621-xiaomi_mi-router-4a-gigabit-squashfs-sysupgrade.bin

#Length: 6554224/0x640270 (6MB) [application/octet-stream]
#Saving to address 0x80001000

flash erase 0x180000 0x640270
flash write 0x180000 0x80001000 0x640270

```



## 5.刷入Factory.bin(修复5G信号弱问题)

```shell
wget http://192.168.1.2/factory.bin		#下载
flash erase 0x50000 0x10000				#清空
flash write 0x50000 0x80001000 0x10000	#刷入
```

```shell
boot flash 0x180000						#重启
```



重启到breed界面,然后开启环境变量,再重启一次进breed就可以添加环境变量了
在环境变量界面，增加`autoboot.command`字段，值设为`boot flash 0x180000`

## 6.设置中文

系统>软件包>更新列表>筛选(chinese)>luci-i18n-base-zh-cn>安装>刷新浏览器

## 7.其他常见操作

### 进入Breed

断开电源>按住`重置按钮`>插入电源>5秒后松开按钮

浏览器访问`192.168.1.1` 如果显示异常就用浏览器的`无痕/访客模式`访问

### 进入Breed SSH

进入Breed  >telnet 192.168.1.1

### 刷回小米官方固件

进入Breed>固件更新>编程器更新>选择All.bin>只勾选重启


