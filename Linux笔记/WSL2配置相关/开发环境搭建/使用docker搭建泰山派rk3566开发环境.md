## 立创教程的流程  
1. 卸载旧版本  

        sudo apt-get remove docker docker-engine docker.io containerd runc  
2. **安装docker**  
使用阿里镜像源，更新 apt 包索引并安装相关软件包以允许 apt 通过 HTTPS 下载安装

        sudo apt-get update
        sudo apt-get install ca-certificates curl gnupg lsb-release  
    在目录/etc/apt/keyrings中添加阿里云的Docker **GPG密钥** ：docker.gpg  

        sudo mkdir -p /etc/apt/keyrings
        curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg  
    添加阿里云的Docker源

        echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null  
    **安装Docker engine**  

        sudo apt-get update
        sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin  
    验证安装完成  

        sudo docker -v
        # 打印出版本号
        Docker version 24.0.2, build cb74dfc  
    将当前用户添加到 docker 用户组

        sudo usermod -aG docker $USER     #将当前用户添加到 docker 组，$USER是环境变量
        newgrp docker                     # 立即切换到 docker 组