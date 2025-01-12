### 一、编译流程
#### 安装ncurses库

        sudo apt-get install libncurses5-dev
#### 1、创建目录来存放uboot相关代码  
在`/home/$USER/linux/uboot/alientek_uboot`中存放**正点原子提供的 uboot 源码**  

        uboot-imx-2016.03-2.1.0-g0ae7e33-v1.7.tar.bz2
    存放到alientek_uboot文件夹下  
    解压

        tar -vxjf uboot-imx-2016.03-2.1.0-g0ae7e33-v1.7.tar.bz2
#### 2、编译uboot  
**第一条指令：**

    make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean
`ARCH=arm` 设置目标为 arm 架构，`CROSS_COMPILE`指定所使用的交叉编译器，就是使用之前安装的Linaro GCC。  
相当于`make distclean`，目的是清除工程，一般在第一次编译的时候最好清理一下工程  
**第二条指令：**

    make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- mx6ull_14x14_ddr512_emmc_defconfig
相当于`make mx6ull_14x14_ddr512_emmc_defconfig`，配置文件位`mx6ull_14x14_ddr512_emmc_defconfig`  
    **第三条指令：**

    make V=1 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j16
相当于`make -j16`，也就是使用16核来编译 uboot。
### 二、烧写uboot
编译完成得到的文件中，**u-boot.bin** 就是编译出来的 uboot二进制文件。    
uboot是个裸机程序，因此需要在其前面加上头部(IVT、DCD等数据)才能在I.MX6U
上执行，**u-boot.imx**文件就是添加头部以后的 u-boot.bin，u-boot.imx就是最终要烧写到开发板中的uboot镜像文件

### 三、编写shell脚本来方便uboot编译
在`/home/$USER/linux/uboot/alientek_uboot`中新建`mx6ull_alientek_emmc.sh `，写入以下内容：

    #!/bin/bash
    make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean
    make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- mx6ull_14x14_ddr512_emmc_defconfig
    make V=1 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j16
即编译uboot流程中执行的功能相当于`make distclean` `make mx6ull_14x14_ddr512_emmc_defconfig` `make -j16`的这三条指令   
后续可以在alientek_uboot目录中执行这个脚本来快捷编译uboot

    chmod +x mx6ull_alientek_emmc.sh //给这个脚本赋予可执行权限
    ./mx6ull_alientek_emmc.sh //执行这个脚本

    