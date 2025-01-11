### 1、关闭所有子系统，将需要迁移的Linux导入到指定目录  

        wsl --export Ubuntu D:/ubuntu.tar

### 2、注销现有Linux子系统  
        wsl --unregister Ubuntu

### 3、将到处的ubuntu.rar安装的指定盘    
        wsl --import Ubuntu D:\ubuntu\ D:\ubuntu.tar --version 2  
        wsl -d ubuntu  
### 4、修改默认启动模式  
        vim /etc/wsl.conf  
添加  

        [user]  
        default=user_name   

**添加root模式默认密码**  

        sudo passwd root
### 3、重新启动  
