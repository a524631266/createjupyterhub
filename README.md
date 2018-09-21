# createjupyterhub
使用Anaconda3-5.1.0-Linux-x86_64.sh 版本 安装并开启配置指南
安装的权限在root下

1. 本次安装版本为 anaconda 5.1.0 ,其他的版本不保证能够顺利安装并启动  
wget https://repo.anaconda.com/archive/Anaconda3-5.1.0-Linux-x86_64.sh

2. bash Anaconda3-5.1.0-Linux-x86_64.sh
    这里要注意,路径需要安装在其他用户可访问的地方
    我默认安装地点
    ```
        /home/anaconda3
    ```
    **不要**安装在root默认地方(/root/anaconda3),
    因为在后期开启spawner用户(其他非admin用户)的时候会出现无法访问jupyterhub/jupyterhub-singleuser的命令
    因为默认系统安装的这两个命令是安装在 /root/anaconda3/bin/**下,一般用户是
    无法通过/root/目录访问的,即使给这两个命令附加
    ```
     [root]$ chmod u+s,o+s filename..
     or
     [root]$ ln -s root/anaconda3/bin/filename /usr/bin.filename

    ```
    也无济于事,均无效
3. 使指令生效
    ```
     [root]$  source ~/.bashrc
     or -------------------------------------------
     [root]$  vim ~/.bashrc
        在文件结尾添加
        # added by Anaconda3 installer
        export PATH="/home/anaconda3/bin:$PATH"
     [root]$ source ~/.bashrc
     ----------------------------------------------
    ```
4. 安装好anaconda3之后,修改pip/conda指令,防止与其他版本(python2)指令混淆
    ```
    [root]$ cd /home/anaconda3/bin
    [root]$ mv pip rootconda3pip
    [root]$ mv conda rootconda3
    ```
5. 安装jupyterhub
    ```
     [root]$ rootconda3 install -c conda-forge jupyterhub
    ```
    这里我们默认选择频道 conda-forge 中的 jupyterhub版本,看版本号,尤其是
    ornado版本需要4.5.3,如果安装的是5.x版本的,会出现找不到initial的情况
    ```
    [root]$ rootconda3 list | grep tornado
        tornado    4.5.3  py36_0  
    [root]$ rootconda3 list | grep jupyterhub
        jupyterhub  0.8.1 py36_0   conda-forge
    ```
    
6. 我们要设置统一的jupyterhub配置文件,我们习惯放在/etc/jupyterhub中,并生成默认的 jupyterhub默认文件
    ```
     [root]$ cd /etc
     [root]$ mkdir jupyterhub
     [root]$ cd jupyterhub
     [root]$ jupyterhub --generate-config
     [root]$ vim jupyterhub_config.py
        配置要点,根据网络的配置经常出现这个那个的问题,最后在groups_whitelist中添加jupyterhub用户组
     [root]$ addgroup jupyterhub
    ```
    文件地址在本项目中[jupyterhub文件](#jupyterhub/jupyterhub_config.py)

7. 创建用户,并给用户创建组jupyterhub
    文件地址已经写好放在项目中的创建用户脚本中