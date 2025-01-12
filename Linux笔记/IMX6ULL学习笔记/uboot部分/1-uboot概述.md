### 一、U-Boot简介：
#### uboot: Universal Boot Loader  
芯片上电以后先运行一段
`bootloader`程序，先初始化DDR等外设，然后将Linux内核从flash(NAND，
NOR FLASH， SD， MMC 等)拷贝到 DDR 中，最后启动 Linux 内核。    
最主要的工作就是**启动Linux内核**，bootloader 和 Linux 内核的关系
就跟 PC 上的 BIOS 和 Windows 的关系一样。

### 二、要用的uboot有哪些
#### 能接触到的uboot：  
1. uboot官方的 uboot 代码
2. 半导体厂商的 uboot 代码
3. 开发板厂商的 uboot 代码

#### 要用哪些uboot：  
1. **购买第三方开发板，使用开发板厂商提供的uboot**  
一般开发板上的外设驱动都由开发板厂商移植好了，可以直接使用uboot
2. **在开发板上移植半导体厂商提供的 uboot，即uboot移植**  
很多外设驱动可能不支持，需要自己移植