# 3、ECS+OSS搭建个人网盘
阿里云实验链接: <a href="https://developer.aliyun.com/adc/scenario/43c2957814ab40a0917e482f16780cff?spm=a2c6h.13858375.devcloud-scene-list.4.31304090Dv3l9o">使用ECS和OSS搭建个人网盘</a>

本文假设您已完成之前课程，并已创建`ECS服务器`及`对象存储(OSS)`，并为`RAM用户`分配OSS所需权限
且已经保存 `accessKeyId` 和 `accessKeySecret`

## 1、登录ECS控制台
#### 点击以下链接进入`ECS控制台` {id="ecs_2"}
#### <a href="https://ecs.console.aliyun.com/home">ECS控制台</a>
![](image1.png){width=720}

#### 配置安全组，放行相应端口
#### 我们需要放行 `80、443、22、3389、5212`
#### 点击左侧 `安全组`
![](image2.png){width="720"}
#### 若您的安全组为空，请点击自行创建

### 点击`手动添加`
![](image3.png)
#### 在端口范围内填写即可，每个端口间使用逗号隔开

#### `源` 可选 `我的IP` 或者 `所有网络`，此处选择`所有网络`
![](image4.png)

#### 最后点击右侧`保存`即可

## 2、安装 `cloudreve`

```console
wget https://labfileapp.oss-cn-hangzhou.aliyuncs.com/cloudreve_3.3.1_linux_amd64.tar.gz
tar -zxvf cloudreve_3.3.1_linux_amd64.tar.gz
chmod +x ./cloudreve
./cloudreve
```

#### 请记录下显示出的账号密码，下文需要使用

#### 返回`ECS控制台`查看ECS`公网IP`地址
![](image6.png)
#### 使用你的ECS公网IP地址+端口号5212 {id="ecs-ip-5212_1"}

#### 在浏览器地址栏粘贴回车即可

#### 例如 `169.254.32.12:5212`

![](image7.png){width="720"}

#### 上文地址不可访问，请以实际为准

#### 使用上一步得到的账号密码进行登录即可

#### 顺利登录后，在ECS终端 `Ctrl+C` 停止该进程

## 3、 安装`ossfs`工具
```console
wget https://gosspublic.alicdn.com/ossfs/ossfs_1.80.6_centos8.0_x86_64.rpm
```

#### 以下步骤包括删除原有源并执行更新

```console
rm -f /etc/yum.repos.d/*
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo
yum clean all && yum makecache
yum install -y ./ossfs_1.80.6_centos8.0_x86_64.rpm
```

#### 下一步，点击进入OSS控制台，查看 `Bucket 列表`

#### <a href="https://oss.console.aliyun.com/bucket">Bucket 列表</a> {id="bucket_1"}
![](image8.png)

#### 拼接命令
```Console
echo Bucket名称:上节课用到的RAM账户AccessKeyId:RAM账户AccessKeySecret > /etc/passwd-ossfs

```

#### 例如: 
```Console
echo abc:fsdafsfdsfdsfs:gdfgfdgfdgerdf > /etc/passwd-ossfs

```

#### 添加执行权限
```Console
chmod 640 /etc/passwd-ossfs

```

#### 创建挂载点
```Console
mkdir oss
```


#### 请在 Bucket 控制台查看 `Endpoint`
#### 点击 Bucket 名称后，在靠近左侧部分点击`概览`
![](image9.png)

#### 在页面底部即可看到`Endpoint`
#### 此处分为内网和外网
![](image10.png)
#### 若您的ECS与OSS在同一地域，例如都在南京，请使用内网，若不在同一地域，请使用公网

#### 例如: 

```Console
ossfs Bucket名称 oss -o url=oss-cn-shanghai.aliyuncs.com
```

#### 请将 Bucket名称 替换为你的Bucket名称 

#### `oss-cn-shanghai.aliyuncs.com` 替换为你所查询到的`Endpoint`
```Console
df -h
```

#### 查看是不是有`ossfs`字样

#### 创建开机自启
```Console
vim /etc/init.d/ossfs
```

#### 输入`i`进入编辑模式
```Console
ossfs BucketName /root/oss -o url=Endpoint -oallow_other
```
#### `BucketName` 、 `Endpoint` 与上文一致

#### 修改完成以后，按下`ESC`输入`:wq` 保存即可
```Console
chmod a+x /etc/init.d/ossfs
chkconfig ossfs on
```


## 4、修改云盘存储位置至`ossfs`
```Console
./cloudreve
```
#### 再次在浏览器地址栏输入`ECS公网IP地址:5212`

#### 使用之前的用户名密码登录
![](image11.png)

#### 点击右上角头像，`管理面板`
![](image12.png)
#### `存储策略`
![](image13.png)
#### `添加存储策略`，选择 `本机存储`
![](image14.png)
#### 存储目录修改为 `oss/{uid}/{path}`
![](image15.png)
#### 最后的`存储策略名`更改为`oss`即可 ，记得保存 {id="oss_1"}
![](image16.png)
#### 点击`用户组`
![](image17.png)
#### 点击管理员右侧的编辑，将`存储策略`更改为`oss`
![](image18.png)
#### 最后点击页面底部的`保存`即可
#### 点击右上角`主页`
![](image19.png)
#### 拖放文件上传

#### 回到ECS命令行 {id="ecs_1"}
```console
cd /root/oss/1
ls
```

#### 根据要求截图即可


#### `ossfs`软件包安装 {collapsible="true" id="ossfs_1"}
<a href="https://help.aliyun.com/zh/oss/developer-reference/install-ossfs?spm=a2c4g.11186623.help-menu-31815.d_5_3_6_0.4ce03fadCT89Nn&scm=20140722.H_2841059._.OR_help-T_cn~zh-V_1
"></a>

