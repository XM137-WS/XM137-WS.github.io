# 3、ECS+OSS搭建个人网盘
阿里云实验链接: <a href="https://developer.aliyun.com/adc/scenario/43c2957814ab40a0917e482f16780cff?spm=a2c6h.13858375.devcloud-scene-list.4.31304090Dv3l9o">使用ECS和OSS搭建个人网盘</a>

本文假设您已完成之前课程，并已创建ECS服务器及对象存储(OSS)，并为RAM用户分配OSS所需权限
且已经保存 accessKeyId 和 accessKeySecret

## 1、登录ECS服务器
![](image1.png){width=720}
点击以下链接进入ECS控制台
<a href="https://ecs.console.aliyun.com/home"></a>

点击选择你的ECS服务器，笔者两个实例，选择第二个是因为有公网

下一步，我们需要配置安全组，放行相应端口
我们需要放行 80、443、22、3389、5212

点击左侧 安全组 
若您安全组为空，请自行创建
点击手动添加
端口范围内填写即可，每个端口间使用逗号隔开

源 可选 我的IP 或者 所有网络，此处选择所有网络
最后点击右侧保存即可

使用远程连接登录ECS 


## 2、下载 cloudreve 安装包
wget https://labfileapp.oss-cn-hangzhou.aliyuncs.com/cloudreve_3.3.1_linux_amd64.tar.gz
tar -zxvf cloudreve_3.3.1_linux_amd64.tar.gz
chmod +x ./cloudreve
./cloudreve

请记录下显示出的账号密码，下文需要使用

返回ECS控制台查看ECS公网IP地址
使用你的ECS公网IP地址+端口号5212
在浏览器地址栏粘贴回车即可
例如
169.254.32.12:5212
上文地址不可访问，请以实际为准

使用上一步得到的账号密码进行登录即可

顺利登录后，在ECS远程连接的终端 Ctrl+C 停止该进程

对于本班大部分来说，都是CentOS7
所以直接复制粘贴即可

wget https://gosspublic.alicdn.com/ossfs/ossfs_1.80.6_centos8.0_x86_64.rpm

以下步骤将删除原有源并执行更新


rm -f /etc/yum.repos.d/*
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo
yum clean all && yum makecache

安装
yum install -y ./ossfs_1.80.6_centos8.0_x86_64.rpm

下一步，点击以下进入OSS控制台，查看 Bucket 列表

<a href="https://oss.console.aliyun.com/bucket">Bucket 列表</a>

复制 Bucket 名称

返回终端

echo Bucket名称:上节课用到的RAM账户AccessKeyId:RAM账户AccessKeySecret > /etc/passwd-ossfs

例如: 
echo abc:fsdafsfdsfdsfs:gdfgfdgfdgerdf > /etc/passwd-ossfs

执行
chmod 640 /etc/passwd-ossfs

mkdir oss

请在 Bucket 控制台查看 Endpoint

点击 Bucket 名称后，在靠近左侧部分点击概览
在页面底部即可看到Endpoint

此处分为内网和外网
若您的ECS与OSS在同一地域，例如都在南京，请使用内网，若不在同一地域，请使用公网

例如: ossfs Bucket名称 oss -o url=oss-cn-shanghai.aliyuncs.com

请将 Bucket名称 替换为你的Bucket名称 oss-cn-shanghai.aliyuncs.com 替换为你所查询到的Endpoint

df -h
查看是不是有ossfs字样

## 创建开机自启
vim /etc/init.d/ossfs
输入i进入编辑模式
ossfs BucketName /root/oss -o url=Endpoint -oallow_other
BucketName 与上文一致
Endpoint 也与上文一致

修改完成以后，按下ESC
输入:wq
保存即可
chmod a+x /etc/init.d/ossfs
chkconfig ossfs on

## 最后配置
./cloudreve
在浏览器地址栏输入ECS公网IP地址:5212

使用之前的用户名密码登录

点击右上角头像，管理面板

存储策略

添加存储策略

选择 本机存储

存储目录修改为 oss/{uid}/{path}

最后的存储策略名更改为oss即可


点击用户组
点击管理员右侧的编辑

将存储策略更改为oss

最后点击页面底部的保存即可

点击右上角主页
拖放文件上传

回到ECS命令行
cd /root/oss/1
ls
查看文件
