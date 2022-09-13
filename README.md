# 自动构建斐讯N1 OpenWrt固件脚本

# Automatically Build OpenWrt Firmware for PHICOMM N1

## 使用方法

1. Linux环境，推荐使用Ubuntu 18.04 LTS
2. 编译好待构建的OpenWrt固件
   编译N1固件的配置如下：
   ``` 
   Target System (QEMU ARM Virtual Machine)  --->
   Subtarget (ARMv8 multiplatform)  --->
   Target Profile (Default)  --->
   ```

3. 将你编译好的固件拷贝到OpenWrt目录( 可以复制多个固件到此 )
4. 使用sudo执行脚本  
   `sudo ./make` 
5. 按照提示操作，如，选择你要制作的固件、选择内核版本、设置ROOTFS分区大小等  
   如果你不了解这些设置项，请直接回车即可，或者直接执行  
   `sudo ./make -d`
6. 等待构建完成，默认输出文件夹为out
7. 写盘启动，写盘工具推荐 [Etcher](https://www.balena.io/etcher/)

**注意**：  
1、待构建的固件格式只支持rootfs.tar[.gz]、 ext4-factory.img[.gz]、root.ext4[.gz] 6种，推荐使用rootfs.tar.gz格式  
2、默认不会清理out目录，有需要的手动删除，或者使用 `sudo ./make -c` 清理  
3、一键安装到emmc命令为：  
   `cd /root && ./install.sh`

## 特别说明

* 目录说明
   ```
   ├── armbian                               armbian相关文件
   │   └── phicomm-n1                        设备文件夹
   │       ├── boot-common.tar.gz            公有启动文件
   │       ├── firmware.tar.gz               armbian固件
   │       ├── kernel                        内核文件夹，在它下面添加你的自定义内核
   │       │   ├── 4.18.7                    4.18.7-aml-s9xxx @XQ7
   │       │   ├── 4.19.106                  4.19.106-amlogic-flippy-27o @flippy
   │       │   └── 5.4.20                    5.4.20-amlogic-flippy-25+ @flippy
   │       └── root                          rootfs文件夹，在它下面添加你的自定义文件
   │           ├── etc                       /etc 文件夹
   │           └── root                      /root 文件夹
   ├── LICENSE                               license
   ├── make                                  构建脚本
   ├── openwrt                               固件文件夹(to build)
   ├── out                                   固件文件夹(build ok)
   ├── tmp                                   临时文件夹
   └── README.md                             readme, current file
   ```

* 使用参数
   * `-c, --clean`，清理临时文件和输出目录
   * `-d, --default`，使用默认配置来构建固件( openwrt下的第一个固件、构建所有内核、ROOTFS分区大小为512m )
   * `--kernel`，显示kernel文件夹下的所有内核
   * `-k=VERSION`，设置内核版本，设置为 `all` 将会构架所有内核的固件
   * `-s, --size=SIZE`，设置ROOTFS分区大小，不要小于256m
   * `-h, --help`，显示帮助信息
   * examples：  
   `sudo ./make -c`，清理文件  
   `sudo ./make -d`，使用默认配置  
   `sudo ./make -k 4.19.106`，将内核版本设置为 `4.19.106`  
   `sudo ./make -s 256`，将ROOTFS分区大小设置为256m  
   `sudo ./make -d -s 256`，使用默认，并将分区大小设置为256m  
   `sudo ./make -d -s 256 -k 4.19.106`，使用默认，并将分区大小设置为256m，内核版本设置为 `4.19.106 `

* 自定义
   * 使用自定义内核  
     参照内核文件夹( `armbian/phicomm-n1/kernel/xxx` )下的文件提取kernel.tar.gz、modules.tar.gz

   * 添加自定义文件  
      向armbian/phicomm-n1/root目录添加你的文件

      **注意**：添加的文件应保持与ROOTFS分区目录结构一致
