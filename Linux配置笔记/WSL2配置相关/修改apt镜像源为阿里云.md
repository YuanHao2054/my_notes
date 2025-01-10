### 1、备份原版镜像源到统一目录  
        sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

### 2、进入apt镜像源列表文件  
        sudo vim /etc/apt/sources.list
**删除原本的内容**  
替换为以下内容  **对应版本为Ubuntu22.04** 其他版本需要对应的镜像源

        ## Note, this file is written by cloud-init on first boot of an instance
        ## modifications made here will not survive a re-bundle.
        ## if you wish to make changes you can:
        ## a.) add 'apt_preserve_sources_list: true' to /etc/cloud/cloud.cfg
        ##     or do the same in user-data
        ## b.) add sources in /etc/apt/sources.list.d
        ## c.) make changes to template file /etc/cloud/templates/sources.list.tmpl
        
        # See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
        # newer versions of the distribution.
        deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted
        deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted
        
        ## Major bug fix updates produced after the final release of the
        ## distribution.
        deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted
        deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted
        
        ## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
        ## team. Also, please note that software in universe WILL NOT receive any
        ## review or updates from the Ubuntu security team.
        deb http://mirrors.aliyun.com/ubuntu/ jammy universe
        deb-src http://mirrors.aliyun.com/ubuntu/ jammy universe
        deb http://mirrors.aliyun.com/ubuntu/ jammy-updates universe
        deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates universe
        
        ## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
        ## team, and may not be under a free licence. Please satisfy yourself as to
        ## your rights to use the software. Also, please note that software in
        ## multiverse WILL NOT receive any review or updates from the Ubuntu
        ## security team.
        deb http://mirrors.aliyun.com/ubuntu/ jammy multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ jammy multiverse
        deb http://mirrors.aliyun.com/ubuntu/ jammy-updates multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates multiverse
        
        ## Uncomment the following two lines to add software from the 'backports'
        ## repository.
        ## N.B. software from this repository may not have been tested as
        ## extensively as that contained in the main release, although it includes
        ## newer versions of some applications which may provide useful features.
        ## Also, please note that software in backports WILL NOT receive any review
        ## or updates from the Ubuntu security team.
        deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
        deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
        
        ## Uncomment the following two lines to add software from Canonical's
        ## 'partner' repository.
        ## This software is not part of Ubuntu, but is offered by Canonical and the
        ## respective vendors as a service to Ubuntu users.
        # deb http://archive.canonical.com/ubuntu jammy partner
        # deb-src http://archive.canonical.com/ubuntu jammy partner
        deb http://mirrors.aliyun.com/ubuntu jammy-security main restricted
        deb-src http://mirrors.aliyun.com/ubuntu jammy-security main restricted
        deb http://mirrors.aliyun.com/ubuntu jammy-security universe
        deb-src http://mirrors.aliyun.com/ubuntu jammy-security universe
        deb http://mirrors.aliyun.com/ubuntu jammy-security multiverse
        deb-src http://mirrors.aliyun.com/ubuntu jammy-security multiverse  
保存退出  

### 更新列表  
                sudo apt update
