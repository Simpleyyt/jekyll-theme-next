# Windows包管理器-winget101

嗨，你好读者，我是ktd（开拓地）,
让我们来入门 "winget" 吧，文章半路已经有结果了，
标题改成微软商店下载winget也差不多
仔细安装步骤就能学会使用Windows winget了！文章略精简版

## 首先，什么是包管理器？
包管理器是
可以使用命令行指令，
操作后，一键下载应用仓库的应用，

### 如何下载winget？
准备环境-微软商店入门教程
win+x 打开菜单打开Windows PowerShell
那么让我们打开微软应用商店

### 找不到微软商店？
使用运行 <ktd> win+R<ktb> 输入
    ms-windows-store:


### 没有微软商店？
使用"Windows PowerShell" 复制输入

    Get-AppxPackage -allusers Microsoft.WindowsStore | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register “$($_.InstallLocation)\AppXManifest.xml”}


回车，没错，微软商店回来了
为了方便将微软商店固定到任务栏

### 无法加载微软商店情况？
设置里临时把lan广域网关闭就可以了

### 需要注册微软账号吗？
非强制性的


搜索“winget”中文显示的叫"应用安装程序"
这中文翻译挺好的，但反抽象了吧(悲)

安装好回到Windows PowerShell
输入 "winget"
可以看到winget成功运行




### 实践教程使用winget下载软件
那么接下来就是见证包管理器效率的时刻，
通过winget下载软件

### 如何使winget有界面
github上刚好有一个项目，同时是有汉化，
    https://github.com/marticliment/WingetUI


### 打开shell(文章前面有提到)输入winget指令
    winget install wingetui

### 恭喜你现在入门winget包管理器


#### 显示系统安装软件的列表
    winget list [&&&]
#### 搜索应用
    winget search [@@@]

#### 主题-将下载条设置为彩虹rainbow
    winget install Musescore.Musescore --rainbow


## 延伸参考(急，写到这里看到的，又又写重复了)
包管理器 winget 使用指南
延伸的图文博文看这篇
<https://www.lifeee.top/posts/36943.html>

顺带看了下热夏的telegram推荐
<https://www.lifeee.top/posts/20469.html>




### 最后推荐一些遇到的扩张类网站

* wiget软件源:
https://winget.run/
* Scoop-舀：
https://scoop.sh/#/
一次安装和更新所有程序：
* https://ninite.com/
推荐好用、优秀的 Windows 应用：
* https://github.com/stackia/best-windows-apps

##### 结尾
最后赞美下民间汉化组，
看番的时候就收益匪浅，
现在看github项目还能享受汉化









