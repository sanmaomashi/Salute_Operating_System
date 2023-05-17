# MacOS中node.js安装与卸载指南

本篇介绍软件包安装和包管理器安装的方式。

npm自node.js v0.6.x版以后就内嵌在node.js中，所以安装node.js就可以了。

### 软件包方式安装

> MacOS系统推荐使用安装包安装，快速便捷。

登录[node.js官网](https://nodejs.org/en/)下载 pkg 安装包。这里如果下载慢可以进入到[中文页面]( https://nodejs.org/zh-cn/download/)中下载相应安装包。

![image-20220528221430225](/Users/zhougaofeng/Desktop/Salute_/Salute_MacOS/img/7.jpg)

下载之后的pkg安装包如下：

![image-20220528221914543](/Users/zhougaofeng/Desktop/Salute_/Salute_MacOS/img/8.jpg)

双击打开 node-v16.15.0.pkg 文件：

![image-20220528222043649](/Users/zhougaofeng/Desktop/Salute_/Salute_MacOS/img/9.jpg)

点击 **继续** 进行安装， 

![image-20220528222118014](/Users/zhougaofeng/Desktop/Salute_/Salute_MacOS/img/10.jpg)

再次点击 **继续** 

![image-20220528222151534](/Users/zhougaofeng/Desktop/Salute_/Salute_MacOS/img/11.jpg)

点击 **同意** 继续下一步：

![image-20220528222222331](/Users/zhougaofeng/Desktop/Salute_/Salute_MacOS/img/12.jpg)

点击 **安装** ，输入您的密码进行安装。

![image-20220528222317028](/Users/zhougaofeng/Desktop/Salute_/Salute_MacOS/img/13.jpg)

稍等片刻。 出现如下页面就标志着安装完成。

![image-20220528222406471](/Users/zhougaofeng/Desktop/Salute_/Salute_MacOS/img/14.jpg)

安装好后在终端执行 `node -v` 和 `npm -v`，显示版本信息，标志安装成功！

```shell
node -v 
npm -v
```

![image-20220528222553784](/Users/zhougaofeng/Desktop/Salute_/Salute_MacOS/img/15.jpg)

### 包管理器方式安装

MacOS中包管理一般使用 Homebrew 进行安装软件。 

打开终端，

```shell
brew install node
```

验证是否安装成功





