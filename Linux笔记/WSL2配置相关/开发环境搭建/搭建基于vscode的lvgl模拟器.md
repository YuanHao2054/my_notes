#### 前置依赖  
* vscode安装插件: C/C++、C/C++ Extension Pack、C/C++ Runner  
* wsl配置ssh密钥到GitHub账号，确保克隆库成功率高  
* wsl安装cmake、gdb、gcc  
```
sudo apt install cmake
sudo apt install gdb

sudo apt install gcc
```  
#### 一、创建目录，克隆对应的库  
在创建/home/yuanhao/lvgl_workspace目录，后续的操作在里面进行  
1. **安装显卡驱动SDL2**  
        在 Linux 上，可以通过终端轻松安装 SDL2：

        查找 SDL2 的当前版本： apt-cache search libsdl2（例如 libsdl2-2.0-0）

        安装 SDL2： sudo apt-get install libsdl2-2.0-0（用找到的版本替换）

        安装 SDL2 开发包： sudo apt-get install libsdl2-dev

        如果尚未安装构建基础工具： sudo apt-get install build-essential

2. 拉取lv_port_pc_vscode  

        git clone https://github.com/lvgl/lv_port_pc_vscode  
3. 进入该目录，删除lvgl文件夹，拉取lvgl库，创建build文件夹  

        cd lv_port_pc_vscode
        rm lvgl/ -rf
        git clone https://github.com/lvgl/lvgl.git
        cd lvgl
        git status
        （根据lv_port_pc_vscode需要的lvgl版本提示下载及切换版本）
        git checkout 8691574
        cd ..
        mkdir build
    lvgl库拉取失败可以用ssh拉取，成功率更高  
    **需要的lvgl版本在lv_port_pc_vscode的GitHub主页查看**
4. 在build文件夹里进行项目编译  
进入build目录  

        cmake ..  
        make    
5. 编译成功后进行调试
vscode**通过文件打开工作区**  
打开**simulator.code-workspace**  
vscode左侧栏打开调试，运行**Debug LVGL demo with gdb**



