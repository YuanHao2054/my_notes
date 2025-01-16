### 基础命令  
1. 查看git配置信息

        git config --list  
2. 配置用户名和用户邮件 

        git config --global user.name "XXXX"
        git config --global user.email  "XXXXXXXXXX"    
3. 配置ssh密钥到GitHub  
***见  配置ssh密钥到GitHub.md***  
4. 验证Git是否能够配置  

        ssh git@github.com  
# 使用git管理代码  
### 1、初始化git仓库  
在项目目录中执行

        git init 
为这个库创建一个分支，并创建.git文件夹

        hint: Using 'master' as the name for the initial branch. This default branch name
        hint: is subject to change. To configure the initial branch name to use in all
        hint: of your new repositories, which will suppress this warning, call:
        hint: 
        hint:   git config --global init.defaultBranch <name>
        hint: 
        hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
        hint: 'development'. The just-created branch can be renamed via this command:
        hint: 
        hint:   git branch -m <name>  
为所新仓库配置初始分支的名称

        git config --global init.defaultBranch <name>  
重命名这个分支

        git branch -m <name>
        例如，将分支重命名为 main：
        git branch -m main  
删除本地的仓库：删除掉.git文件夹就行  

        rm -rf .git
        或者直接在资源管理器中删除
### 2、将代码提交到仓库
**提交到仓库前要先把代码放入暂存区**  
放入指定文件到暂存区

        git add <文件名> 
放入项目的所有文件到暂存区

        git add .
**将暂存区中的代码提交到仓库**  

        git commit -m "备注"  
设置不需要提交的文件（忽略某文件）  
1. 创建.gitignore文件

        touch .gitignore  
2. 在.gitignore文件放入要不需要提交的文件的名称  

可以把add和commit放在一起完成

        git commit -a -m "备注"
        git commit -am "备注"
### 3、查看提交信息  

        git log  
        Author: yuanhao <2054190139@qq.com>
        Date:   Fri Jan 3 11:07:27 2025 +0800

        初始化  
查看更详细的信息  
1. 查看修改了什么文件  

        git log --stat


        commit 9b568e994ed01d4577aa9b96137fa9d1283aaf84 (HEAD -> master)
        Author: yuanhao <2054190139@qq.com>
        Date:   Fri Jan 3 11:07:27 2025 +0800

        初始化

        src/FreeRTOS_Posix_Port.c |  92 ++++++++++++++++++++++++++++++++++++++++++++++++++++
        src/freertos_main.cpp     | 179 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
        src/main.c                | 137 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
        src/mouse_cursor_icon.c   |  34 ++++++++++++++++++++
        src/mygui.c               |   9 ++++++
        src/mygui.h               |  20 ++++++++++++
        6 files changed, 471 insertions(+)  
2. 查看某次提交修改了什么
找到commit id 如commit **9b568e994ed01d4577aa9b96137fa9d1283aaf84** (HEAD -> master)  

        git diff <commit id>  

### 3、代码回溯 
使用这两个代码中的任意一个： 

        git reset --hard <commit id>
        git checkout <commit id>

        例如：回退到9b568e994ed01d4577aa9b96137fa9d1283aaf84节点
        git checkout 9b568e994ed01d4577aa9b96137fa9d1283aaf84  
将代码回退到指定节点  

### 4、分支管理
查看分支情况

        git branch
查看当前分支情况 **可以用这个命令来提示自己下一步应该怎么操作**

        git status
        显示当前分支名称、是否有文件未提交（Untracked files)  

创建一个名为"develop"的分支  

        git checkout -b develop(创建并跳转)
        git branch -b develop(创建不跳转)  
切换分支：如切换到master分支  

        git checkout master  
**将其他分支合并到master分支**  
1. 在其他分支修改完代码并commit后，切换到master分支  
2. 使用命令将指定分支合并到master分支  
如：将develop分支合并到master分支  

        git merge develop  
**删除某个分支**  
例如删除develop分支  

        git branch -d develop
        git branch -D develop (换成大写D，表示强制删除，不需要考虑是否合并)

### 5、用GitHub管理代码
**配置个人访问token**  
进入GitHub>点击头像>setting>(页面最下方)Developer setting>personal access tokens，在这里进行tokens管理 

查看本地仓库和哪个远程仓库有联系(fetch/push)

        git remote -v  
#### 用GitHub管理代码库的一般流程   
创建之前先把GitHub默认分支由main改成master 
1. 在GitHub中创建一个仓库，名称就是文件夹的名字
2. 将远程仓库（空仓库）克隆到本地，将代码放入这个文件夹中  
3. commit一次本地的代码到master  
4. 执行

        git push
        在vscode上跳出的框里粘贴个人访问token代码，按提示ENTER两次，即可将master分支上传到GitHub远程仓库  
5. 将指定分支push到远程仓库  

        git push -u origin master    （master分支）
        git push -u origin test       (test分支)  
6. 用远程仓库替换本地仓库  

#### 其他  
1. 更新git本地仓库关联的远程仓库地址  

        git remote set-url origin 新的远程仓库地址
        例如：
        git remote set-url origin https://github.com/YuanHao2054/esp32_Network_learn.git 

        再查看本地仓库关联的远程仓库地址
        git remote -v



  
### 6、git技巧  
#### 1、 **使用.gitignore文件配置提交时要忽略的内容**  
忽略和删除不一样，如果要忽略的文件已经被跟踪，但它并没有从仓库中消失，需要手动清理  
如果需要被忽略的内容以及被提交上去，要先删除版本库中的对应内容：

        git rm -r --cached 1-1AP/build/  


- 忽略仓库根目录中的文件夹：如忽略 privacy文件夹：

        /privacy/  
- 忽略仓库中二级、三级文件夹：如忽悠工程文件夹1-1AP中的build文件夹：

        1-1AP/build/
        1-2STA/build/
        2-1TCPClient/build/
- 忽略仓库中二级、三级文件夹中的内容，但保留文件夹本体：

        1-1AP/build/*
        1-2STA/build/*
        2-1TCPClient/build/*
        同时加上，在对应目录中放一个.gitkeep文件，就能保留目录本身
        !1-1AP/build/.gitkeep
        !1-2STA/build/.gitkeep
        !2-1TCPClient/build/.gitkeep

#### 2、重命名本地分支  
1. **如果还没有推送到远程**，直接使用这个命令来重命名：

        git branch -m oldName newName
2. **如果已经推送到远程**     
- 重命名本地分支  

        git branch -m oldName newName
- 删除远程分支

        git push --delete origin oldName
- 解除原先远程分支的关联关系

        git branch --unset-upstream 
- 上传新的本地分支

        git push origin newName
