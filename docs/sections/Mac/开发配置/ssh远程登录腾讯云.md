# Mac ssh远程登录腾讯云

> 本篇基于腾讯云 ｜ Ubuntu Server 18.04.1 LTS 64bit ｜ macOS Monterey v12.4 进行测试

## 1、ubuntu开启ssh

ubuntu第一次启动的话是默认没有openssh-server服务的，需要我们自行安装，通过腾讯云web连接到服务器

```bash
sudo apt-get update && sudo apt-get install -y openssh-server vim
```
`可选：`腾讯云ubuntu云主机默认用户名是ubuntu，ubuntu 默认没有激活root，如果需要用到root账户登录服务器，需要在root权限下为root设置一下密码

```bash
sudo passwd root
# 设置可以root登录ssh
sudo vim /etc/ssh/sshd_config
# 输入 :/关键词 搜索，回车确定位置后，在输入模式（i）下更改配置后，点击：“ESC”切换到命令行模式，输入“:wq”保存离开，注意去掉注释。
# 将PermitRootLogin prohibit-password改为PermitRootLogin yes 
```

重启ssh服务

```bash
sudo /etc/init.d/ssh restart
```

```bash
# 安装后可通过该条命令查看是否安装成功,如果出现一行结果最后有sshd说明已经开放了这个服务。
ps -e | grep ssh 
# 如果没有sshd，则通过输入以下任意一条命令启动
sudo /etc/init.d/ssh start  
```

## 2、在腾讯后台创建SSH密钥

- 登录 [云服务器控制台](https://console.cloud.tencent.com/cvm/)，这里我以[轻量应用服务器](https://console.cloud.tencent.com/lighthouse/sshkey/index)为例。
- 在左侧导航栏中，单击轻量应用服务器 [**密钥**](https://console.cloud.tencent.com/lighthouse/sshkey/index)，云服务器为 **[SSH密钥](https://console.cloud.tencent.com/cvm/sshkey)**。
- 在密钥管理页面，单击创建密钥

![img](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/16.jpg)

- 若创建方式选择 “创建新密钥对”，请输入密钥名称。
- 若创建方式选择 “使用已有公钥”，请输入密钥名称和原有的公钥信息。

> ⚠️ 因为之前配置过git，所以可以执行以下命令将输出的公钥内容复制到腾讯云，关机实例并绑定，直接配置终端（新建远程连接）即可。

```bash
cat ~/.ssh/id_rsa.pub
```

## 3、下载密钥到本地

![img](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/17.jpg)

这个时候我们mac上就会有一个从腾讯云下载下来的ssh密匙,下载保存的路径自己选择（若加入的是自己之前配置的公钥，跳过此步）

打开终端，给文件设置权限，给文件设置权限为 600

```bash
chmod 600 密钥本地地址/'密钥名称'
# 例如
chmod 600 /Users/zhougaofeng/Downloads/mac_zgf
```

## 4、密钥绑定实例

- 登录 [云服务器控制台](https://console.cloud.tencent.com/cvm/)。
- 将所需要绑定密钥的服务器关机。
- 在左侧导航栏中，单击 **[SSH密钥](https://console.cloud.tencent.com/cvm/sshkey)**。
- 在 SSH 密钥管理页面，勾选需要绑定云服务器的 SSH 密钥，并单击**绑定实例**。

- 在弹出的绑定实例窗口中，选择地域，勾选需绑定的云服务器，单击**确定**。

![img](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/18.jpg)

## 5、mac终端访问

### （1）终端命令行连接

打开电脑终端 输入命令

```bash
ssh -i [私匙的本地路径] [主机名]@[主机地址]
# 例如：ssh -i /Users/zhougaofeng/Downloads/mac_zgf ubuntu@118.xx.xx.xx 
```

![img](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/19.jpg)

### （2）终端自带ssh远程连接（推荐）

如果使用的是腾讯云ssh，并且在腾讯云ssh面板中已经生成了ssh公钥和私钥，那么公钥可以在腾讯云ssh面板加载到服务器中，无需我们手动push。我们需要做的就是把私钥放到mac的.ssh目录下就行了

1. 命令打开ssh目录（如果你电脑没有.ssh文件夹，则创建一个）

```bash
ls -la ~/.ssh
```

![img](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/9.jpg)

2. 下面再把从腾讯云下载的私钥粘贴到该目录下，并在终端新建远程连接

![img](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/21.jpg)

3. 最后连接成功，.ssh目录下多了一个known_hosts文件

![img](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/22.jpg)

如果使用mac建立ssh连接，在mac上生成ssh公钥和私钥，然后把生成的公钥push到服务上。则可以直接填入服务器地址，指定用户名ubuntu后，在终端新建远程连接

![image-20220621094503940](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/20.jpg)

> 总结：推荐适用方式二，虽然第一次使用需要配置一下，但是这样第二次连接服务器就不需要重新输入命令，直接右键终端的新建远程连接使用上一次记录即可

⚠️：使用ssh免密登录后，原来的使用账号密码登录方式便会失效



## Reference

- https://www.jianshu.com/p/711eab05268f

- https://cloud.tencent.com/document/product/213/44971

  