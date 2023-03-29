# （MacOS）Github配置免密登录

> 提示：其他远程仓库的SSHKey配置步骤都类似，例如：**Gitee，GitHub、Gitlab等**。

## 一、引言

Git有两种克隆代码的方式：一种是http，另外一种就是SSH。

用http方式拉取的仓库，每次向GitHub仓库推送代码都要输入Git的账号与密码，进行身份验证才可访问，非常麻烦。那么如何才能避免每次推送代码都要输入账号密码呢？



## 二、免密登陆原理

**（1）免密登录机制**

Git主机间的通信采用的是SSH协议，即`Sercure Shell`协议。

该协议的免密登录机制，要求主机之间采用`SSH-key`，即SSH密钥的方式进行身份验证。

SSH密钥包含“公钥与私钥"，所以我们首先要了解什么是“公钥与私钥”，然后还要理解“公钥与私钥”在免密登录中的作用，即免密登录的工作原理。

**（2）公钥与私钥**

公钥（Public Key）与私钥（Private Key）是通过加密算法得到的一个密钥对（即一个公钥和一个私钥，也就是非对称加密方式）。公钥可对会话进行加密、验证数字签名，只有使用对应的私钥才能解密会话数据，从而保证数据传输的安全性。公钥是密钥对外公开的部分，私钥则是非公开的部分，由用户自行保管。

通过加密算法得到的密钥对可以保证在世界范围内是唯一的。使用密钥对的时候，如果用其中一个密钥加密一段数据，只能使用密钥对中的另一个密钥才能解密数据。例如：用公钥加密的数据必须用对应的私钥才能解密；如果用私钥进行加密也必须使用对应的公钥才能解密，否则将无法成功解密。

**（3）免密码登录的工作原理**

对于免密登录的机制，主要由两部分构成：构建与验证。

- 免密登录构建：
  Git版本库所在的主机上生成一对密钥：公钥与私钥，保存到本地主机中。
  Git版本库将公钥及用户信息（用户名，密码等）保存到GitHub上。
- 免密登录验证：
  1、Git版本库向GitHub发送连接请求，包含Git版本库的用户信息。
  2、GitHub从本地文件中查找是否存连接中包含的用户信息。若不存在：则拒绝访问，若存在：可以访问。
  3、GitHub将加密后的随机字符串，发送给Git版本库。
  4、Git版本库将加密后的随机字符串，使用私钥进行解密。
  5、Git版本库解密后的字符发送给GitHub。
  6、GitHub将接收到的字符串，与本地未加密的字符串进行比较，相同则可以访问，不同则被拒绝。

免密登录原理示意图：

![20](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/8.jpg)

简单的理解：

`SSH Key`也可以简单的理解为你的身份标识，放在GitHub上面标明你是这个项目的一个开发人员，但是别人可以截获，但是你本机上的私钥无法截获，也就保证了每次传输是安全的。



## 三、具体配置

**1. 配置全局的用户名和邮箱（不是必须）**

```shell
git config --global user.name "xxx"
git config --global user.email "xxx.com"
```

**2. 打开命令行终端,验证是否有ssh keys,看下返回的结果中是否已经存在了.pub结尾的文件**

```bash
ls -alh ~/.ssh
```

如果有.pub结尾的文件直接打开，复制到github上的SSH keys，如果没有就继续执行

![10](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/9.jpg)

**2. 使用如下命令生成ssh-keygen密钥,然后一路回车。其中，“邮箱地址”是你的github关联的邮箱。**

```bash
ssh-keygen -t rsa -C "邮箱地址"
```

![11](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/10.jpg)

这个时候在默认路径下就生成了两个文件，公钥和私钥。进入主目录下的.ssh文件夹，公钥为id_rsa.pub，私钥为id_rsa。

![12](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/11.jpg)

执行以下命令查看公钥内容，打开id_rsa.pub文件，复制此内容

```bash
cat ~/.ssh/id_rsa.pub
```

![13](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/12.jpg)

**3. 点击账户头像后的下拉三角，选择'settings'。**

![14](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/13.jpg)

**4. 点击'SSH and GPG keys'，添加ssh公钥。**

![15](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/14.jpg)

**5. 填写标题，粘贴公钥，点击Add SSH key，输入GitHub密码即可**

![16](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/15.jpg)

**6. 验证GitHub是否链接**

```bash
ssh -T git@github.com
```

这样github的ssh就可以链接mac了，可以上传和下载代码了~~



## Reference

- https://baijiahao.baidu.com/s?id=1705605704758775994&wfr=spider&for=pc
