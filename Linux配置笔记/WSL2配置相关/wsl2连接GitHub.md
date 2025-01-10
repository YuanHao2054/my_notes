# 大纲  
* VPN开启全局代理，在设置中找到代理服务器的IP和端口，如IP:127.0.0.1 端口：8081  
* wsl配置为镜像模式，让wsl的ip和主机一致  
* git设置，确保git使用与系统代理相同的端口       
---

#### 1、找到代理的ip和端口    
打开**设置>网络与互联网>代理**
#### 2、wsl开启镜像模式    
两种方法  
1. win11 23H2版本及以上的wsl setting页面 **网络>网络模式>Mirrored**  
2. 手动配置.wslconfig文件  
在C:\Users\username目录下创建.wslconfig文件，输入以下内容：  
```ini
[wsl2]
networkingMode=mirrored
```
#### 3、配置git代理  
确保 Git 使用与系统代理设置相同的端口。可以通过以下命令配置 Git 的代理： 
```  
git config --global http.proxy http://127.0.0.1:1234
git config --global https.proxy http://127.0.0.1:1234
```  
其中**http://127.0.0.1:1234**填入在代理中找到的ip和端口  
验证是否生效  
```
git config --global -l
```  
刷新DNS缓存  
```
ipconfig /flushdns

```

#### 4、https链接替换为GitHub镜像（这样不需要代理）  
进入~/.gitconfig  

    sudo vim ~/.gitconfig  

内容注释掉，换成如下内容：  

    [url "https://mirror.ghproxy.com/github.com"]
            insteadof = https://github.com  
保存退出即可

