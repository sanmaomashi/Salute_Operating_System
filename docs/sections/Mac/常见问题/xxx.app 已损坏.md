# xxx.app 已损坏，无法打开，你应该将它移到废纸篓 / 打不开 xxx，因为它来自身份不明的开发者解决方法



##  引言

macOS 启用了新的安全机制，默认是只允许安装自家【App Store】来源的应用，如果你想安装第三方的应用，那么需要在【系统偏好设置 -> 安全性与隐私 -> 通用】中开启【任何来源】选项，但是 macOS 默认是隐藏了这个设置的，需要用户手动通过终端执行命令行代码来开启。



## 开启任何来源

开启任何来源，先打开 `系统偏好设置 -> 安全与隐私 -> 通用` 选项卡，检查是否已经启用了 `任何来源` 选项。

![任何来源](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/7.jpg)

如果没有这个选项，复制以下面的命令：

```bash
sudo spctl --master-disable
```

打开`终端`，将刚刚复制的命令粘贴到终端中，然后按下键盘的回车键，输入密码。

> 回车后会看见个password后面还有个钥匙图标，在钥匙图标后面输入你自己电脑解锁密码（⚠️ 密码输入过程是不会显示）

再次打开 `系统偏好设置 -> 安全与隐私 -> 通用` 选项卡，可以看到，已成功开启任何来源。

![任何来源](https://raw.githubusercontent.com/SaluteGF/Salute_MacOS/main/img/4.jpg)

> 如果想关闭任何来源，执行如下代码：

```bash
sudo spctl --master-enable
```

到这里一般情况下 85% 的应用都可以安装运行了。



## 绕过公证

有的应用开启了任何来源还是报错，这是因为苹果进一步收缩了对未签名应用的权限，这时候就需要过终端执行命令行代码来绕过应用签名认证了。

打开终端，输入以下命令：

```bash
sudo xattr -rd com.apple.quarantine /Applications/xxxxxx.app
```

将上面的 `xxxxxx`.app 换成你的App名称，比如 `Sketch.app`

```bash
sudo xattr -rd com.apple.quarantine /Applications/Sketch.app
```

或者复制以下命令粘贴到终端后

```bash
sudo xattr -rd com.apple.quarantine 
```

打开Finder（访达），点击左侧的 `应用程序`，将应用拖进终端中，然后按键盘的回车键（return），输入密码，再按回车键，完成。

> 注意 `quarantine `后面必须有个空格

到这里一般情况下 90% 的应用都可以安装运行了



## 应用签名

如果还不行，那就需要对应用进行本地签名操作了！

1. 安装Command Line Tools 工具

打开终端工具输入如下命令：

```bash
xcode-select --install
```

弹出安装窗口后选择`继续安装`，安装过程需要几分钟，请耐心等待。

如果安装的时候提示“不能安装该软件，因为当前无法从软件更新服务器获得”，执行以下步骤：

a. 打开苹果开发者中心：[https://developer.apple.com](https://developer.apple.com/)(苹果开发者中心的服务器不在国内，所以打开会很慢，耐心等待)

b. 点击顶部导航最右边的 `Account`，然后登录自己的 Apple ID

c. 打开开发者下载中心：https://developer.apple.com/download/more/

d. 搜索 command line tools（在搜索框中输入完要按一下回车键）

e. 选择适用于自己 macOS 系统的版本。

- 10.15.x 可以下载 Command Line Tools for Xcode 11.4 及以上版本
- 10.14.x 可以下载 Command Line Tools (macOS 10.14) for xxx，其中包含 macOS 10.14的。
- 10.13.x 可以下载 Command Line Tools (macOS 10.13) for xxx，其中包含 macOS 10.13的。
- xxxxx其它版同以此类推。

f. 下载完成后，安装一下，安装完成后就可以使用啦。



2. 打开终端工具输入并执行如下命令对应用签名：

```bash
sudo codesign --force --deep --sign - (应用路径)
```

> 应用路径：打开访达（Finder），点击左侧导航栏的 `应用程序`，找到相关应用，将它拖进终端命令`- `的后面，然后按下回车即可，注意最后一个 `- `后面有一个空格。

正常情况下只有一行提示，即成功：

```success
/文件位置 : replacing existing signature
```

##### 如遇如下错误：

```error
/文件位置 : replacing existing signature
/文件位置 : resource fork,Finder information,or similar detritus not allowed
```

先在终端执行：

```bash
xattr -cr /文件位置（直接将应用拖进去即可）
```

然后再次执行如下指令即可：

```bash
codesign --force --deep --sign - /文件位置（直接将应用拖进去即可）
```