# 小白级的CocoaPods安装和使用教程


---

百度有很多CocoaPods的安装教程.第一次看的时候,确实有点摸不透的感觉.经过思考,一步一步来实践,前后花了三十几分钟,才顺利使用.=.=所以想了想,我还是写一个小白级的教程吧.细到每一个细节都说明. 让你不用10分钟解决CocoaPods.

##CocoaPods简介
每种语言发展到一个阶段，就会出现相应的依赖管理工具，例如Java语言的Maven，nodejs的npm。随着iOS开发者的增多，业界也出现了为iOS程序提供依赖管理的工具，它的名字叫做：CocoaPods。

CocoaPods项目的源码在Github上管理。该项目开始于2011年8月12日，经过多年发展，现在已经成为iOS开发事实上的依赖管理标准工具。开发iOS项目不可避免地要使用第三方开源库，CocoaPods的出现使得我们可以节省设置和更新第三方开源库的时间。

CocoaPods的官网:https://cocoapods.org/

**将官方的ruby源替换成国内淘宝的源**
ruby的软件源rubygems.org因为使用的亚马逊的云服务，所以被墙了，需要更新一下ruby的源.
```
gem sources --remove https://rubygems.org/   //去掉ruby软件源

gem sources -a http://ruby.taobao.org/   //添加淘宝的源

gem sources -l     //查看ruby软件源
```
**安装**
安装方式异常简单, Mac下都自带ruby，使用ruby的gem命令即可下载安装：
```
sudo gem install cocoapods //由于sudo超级权限,所以会填用户密码
```
如果你的gem太老，可能也会有问题，可以尝试用如下命令升级gem:
```
sudo gem update --system
```
开始下载,受网络缘故,需要等待的时间有点久.

如果下载好了,会出现"20 gems installed"

**查询下载进度**

Cocoapods在将它的信息下载到~/.cocoapods目录下，如果你等太久，可以试着cd到那个目录，用du -sh *来查看下载进度。

**pod setup**

pod setup在执行时，会输出
```
Setting up CocoaPods master repo
```

安装好后,会出现"Setup completed"

**使用Podfile**

你看到这里也许会问，CocoaPods为什么能下载AFNetworking呢，而不是下载其他类库呢？这个问题的答案是，有个文件来控制CocoaPods该下载什么。这个文件就叫做“Podfile”（注意，一定得是这个文件名，而且没有后缀）。你创建一个Podfile文件，然后在里面添加你需要下载的类库，也就是告诉CocoaPods，“某某和某某和某某某，快到碗里来！”。每个项目只需要一个Podfile文件。

好吧，废话少说，我们先创建这个神奇的PodFile。在终端中进入（cd命令）你项目所在目录，然后在当前目录下，利用vim创建Podfile，运行：
```
 vim Podfile
```
然后在Podfile文件中输入以下文字：
```
platform :ios, '7.0'

pod "AFNetworking", "~> 2.0"
```
这两句文字的意思是，当前AFNetworking支持的iOS最高版本是iOS 7.0, 要下载的AFNetworking版本是2.0。

其实,
```
platform :ios 

pod 'AFNetworking'
```
这样子就可以了.会自动最新的稳定版本.

vim环境下，保存退出命令是： 
```
:wq
```
有些人没用过vim的.直接打:wq ->Enter ,没反应.

实际是要这样:ESC  -> :wq  -> enter 你会发现,光标已经移到最下面了.

**使用**

然后你将编辑好的Podfile文件放到你的项目根目录中，执行如下命令即可：
```
cd "你的项目根目录"

pod install
```
现在，你的所有第三方库都已经下载完成并且设置好了编译参数和依赖，你只需要记住如下2点即可：

使用CocoaPods生成的.xcworkspace 文件来打开工程，而不是以前的.xcodeproj 文件。

每次更改了Podfile文件，你需要重新执行一次pod update命令。



文／庞大不小（简书作者）
原文链接：http://www.jianshu.com/p/e2f65848dddc
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。




