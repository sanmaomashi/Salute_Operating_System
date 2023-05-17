WeChatTweak  v1.2.2



WeChatTweak 是一款开源的 macOS 微信客户端小助手，相比于之前的微信小助手，Tweak 的功能就比较简单直接，主要包括防撤回、多开、免二次认证登录和消息处理增强。



安装教程

1. 双击解压下载的「WeChatTweak-macOS-1.2.2.zip」
2. 打开「终端」App （在启动台-其他文件夹中）
3. 在「终端」App 中输入 cd ,并敲一个空格，然后把解压的文件夹拖到「终端」 App 中（此步骤是为了获取 WeChatTweak 的路径
4. 在终端中粘贴这段命令并回车执行 xattr -d com.apple.quarantine wechattweak-cli
5. 再将下面的命令粘贴到终端，回车执行chmod +x wechattweak-cli
6. 再将 sudo ./wechattweak-cli --install 复制粘贴到终端，敲回车，提示输入开机密码，在 password 后面输入开机密码，输入的时候不会显示任何字符，直接输，输完敲回车（这一步的目的是执行安装）
7. 然后会发现无法打开「微信」
8. 我们需要重启 Mac ，然后登录并打开微信的设置，有 Tweak ，则大功告成。

问题解决：

1. 屏幕录制失效：

「系统偏好设置-安全性与隐私-屏幕录制」点击 "-" 删除然后再点击 "+"，最后重开微信就好

————————————————
版权声明：本文为CSDN博主「池塘小白」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/u011342720/article/details/122942131
