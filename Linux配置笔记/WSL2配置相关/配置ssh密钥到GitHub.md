#### 1、查看是否已有SSH密钥  
    ls -al ~/.ssh
如果已有id_rsa和id_rsa.pub文件，说明你已经有SSH密钥，可以跳过生成密钥的步骤  
#### 2、生成密钥  
    ssh-keygen -t rsa -b 4096 -C "2054190139@qq.com"  
选择存放位置 

    /home/yuanhao/.ssh/id_rsa  
设置密钥密码短语

    yuanhao@PC-YUANHAO11:~/lvgl_workspace/lv_port_pc_vscode$  ssh-keygen -t rsa -b 4096 -C "2054190139@qq.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/yuanhao/.ssh/id_rsa): 
    Enter passphrase (empty for no passp
    Enter same passphrase again: 
    Your identification has been saved in /home/yuanhao/.ssh/id_rsa
    Your public key has been saved in /home/yuanhao/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:jRIVA5GfpqRrSsul0wXDgTtKBu13GSdiV0Gf6ksg4cU 2054190139@qq.com
    The key's randomart image is:
    +---[RSA 4096]----+
    | . .  o*Bo       |
    |. o +.=..o .     |
    |.. +.+E* .o      |
    | o+.=o+.++       |
    |o. oo*ooS .      |
    |.   ..o+         |
    |  ...o  o        |
    | o.++  . .       |
    |  =+    .        |
    +----[SHA256]-----+  
#### 3、添加SSH密钥到SSH代理  
启动SSH代理  **每次启动系统后要先启动一次ssh代理才能用ssh拉取库**

    eval "$(ssh-agent -s)"    

   
添加SSH密钥到代理 

    ssh-add ~/.ssh/id_rsa  

    yuanhao@PC-YUANHAO11:~/lvgl_workspace/lv_port_pc_vscode$ eval "$(ssh-agent -s)" 
    Agent pid 7627
    yuanhao@PC-YUANHAO11:~/lvgl_workspace/lv_port_pc_vscode$ ssh-add ~/.ssh/id_rsa 
    Enter passphrase for /home/yuanhao/.ssh/id_rsa: 
    Identity added: /home/yuanhao/.ssh/id_rsa (2054190139@qq.com)  

#### 4、将将SSH公钥添加到GitHub    
打开密钥id_rsa.pub，并复制  

    cat ~/.ssh/id_rsa.pub  

    yuanhao@PC-YUANHAO11:~/lvgl_workspace/lv_port_pc_vscode$ cat ~/.ssh/id_rsa.pub 

**将密钥添加到GitHub账户**
* 添加公钥到GitHub：
* 登录到你的GitHub账户。（如果还没有创建GitHub账户的话需要先创建一个账户）
* 点击右上角的头像，选择“Settings”。
* 在左侧菜单中选择“SSH and GPG keys”。
* 点击“New SSH key”按钮。
* 在“Title”中输入一个描述（例如“Ubuntu 20.04 SSH Key”）。
* 在“Key”中粘贴你复制的公钥内容。
* 点击“Add SSH key”按钮。  

#### 5、使用ssh克隆仓库 
例如：  

    git clone git@github.com:CoSaNoVo/scikit-fm.git



