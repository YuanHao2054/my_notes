### 一、uboot常用指令
1. 查看环境变量

        printenv
2. 重启

        reset
3. 运行环境变量中定义的命令

        run
4. 简单的内存读写测试

        mtest 
        例如：
        mtest 80000000 80001000 //测试0X80000000~0X80001000这段内存

### 二、操作uboot环境变量
1. **修改环境变量的值**   
将环境变量 bootdelay 改为 5  

        setenv bootdelay 5  //将uboot启动延时修改为5s，修改的是DRAM中的值
        saveenv //将修改后的环境变量保存到 flash 中
        
    运行结果：环境变量保存到了 MMC(0)中，也就是SD卡

        => setenv bootdelay 5
        => saveenv
        Saving Environment to MMC...
        Writing to MMC(0)... done
        =>

2. **要修改的环境变量的值有空格（不是一个值）**   
用单引号把值包含起来  
例如：

        setenv bootargs 'console=ttymxc0,115200 root=/dev/mmcblk1p2 rootwait rw'
        saveenv
3. **新建环境变量**  
直接使用setenv新建一个环境变量  
例如设置一个author

        setenv author yuanhao
        saveenv
    使用printenv查看

        => setenv author yuanhao
        => saveenv
        Saving Environment to MMC...
        Writing to MMC(0)... done
        => printenv
        author=yuanhao
        baudrate=115200
        board_name=EVK
        board_rev=14X14
        boot_fdt=try
4. **删除环境变量**  
只用使用setenv给要删除的环境变量赋空值就行

        setenv author
        saveenv
    再查看就没有这个变量了
### 三、操作内存
操作DRAM中的内存

### 四、操作网络  
在移植 uboot 的时候一般都要调通网络功能，因为在移植 linux
kernel 的时候需要使用到 uboot 的网络功能做调试  
#### 设置网络相关的环境变量
| 环境变量 | 描述 |
|----------|------|
| ipaddr   | 开发板 IP 地址，可以不设置，使用 dhcp 命令来从路由器获取 IP 地址。 |
| ethaddr  | 开发板的 MAC 地址，一定要设置。 |
| gatewayip| 网关地址。 |
| netmask  | 子网掩码。 |
| serverip | 服务器 IP 地址，也就是 Ubuntu 主机 IP 地址，用于调试代码。 |  

修改环境变量如下： 需要确保uboot和Ubuntu主机的ip在同一局域网下，方便调试

    setenv ipaddr 192.168.1.50
    setenv ethaddr b8:ae:1d:01:00:00
    setenv gatewayip 192.168.1.1
    setenv netmask 255.255.255.0
    setenv serverip 192.168.1.102
    saveenv

也可以用`dhcp`从路由器获取ip  

#### nfs网络文件系统
#### tftp命令

### 五、操作emmc或sd卡
### 六、FAT 格式文件系统操作命令
### 七、EXT 格式文件系统操作命令
### 八、BOOT 操作命令