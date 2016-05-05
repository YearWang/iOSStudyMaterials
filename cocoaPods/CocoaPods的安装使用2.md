# CocoaPods的安装使用


---

##CocoaPods的运行原理:
CocoaPods是强大的第三方框架管理工具,它是将所有的依赖库都放到另一个名为Pods项目中,然后 让主项目依赖Pods项目,这样,源码管理工作都从主项目移到了Pods项目中

Pods项目最终会编译成一个名为libPods.a的文件,主项目只需要依赖这个.a 文件即可。
对于资源文件,CocoaPods提供了一个名为Pods-resources.sh的bash脚本, 该脚本在每次项目编译的时候都会执行,将第三方库的各种资源文件复制到目 标目录中。
CocoaPods通过一个名为Pods.xcconfig的文件来在编译时设置所有的依赖和 参数。

##CocoaPods安装流程:
###准备工作
```
升级电脑的gem(不是必要步骤)
终端中敲下面命令:sudo gem update --system
切换CocoaPods的数据源(最好要做)
移除原来的源:gem sources --remove https:// rubygems.org/
换成淘宝的源gem sources -a http://ruby.taobao.org/
显示现在的源gem sources -l
```
###安装CocoaPods
```
终端中敲下面命令:
sudo gem install cocoa pods
```
###设置 pod 仓库
```
pod setup
```
默认这样更新会比较慢(在国外的网站),可以将文件的托管地址放到国内的网站上,将文件托管的地址从国外托管到国内
```
pod repo remove master
pod repo add master http://git.oschina.net/akuandev/Specs.git
pod repo update
```
只有支持CocoaPods的框架才可以被CocoaPods管理,初始化仓库的目的就是下载所有支持CocoaPods的框架相关的名称和配置信息,在CocoaPods里直接搜索,就可以查看这个框架是不是支持CocoaPods

如何判断一个框架是否支持CocoaPods,只要有XXX.podspec文件就说明支持CocoaPods管理

###测试
```
pod --version
```
显示版本号就说明已经安装好了

##CocoaPods使用说明
###搜索地第三方框架
(这里以SDWebImage框架为例)
```
举例: pod search SDWebImage
```
###搜索到第三方框架以后利用CocoaPods安装第三方框架
利用vim创建Podfile。
```
注意:Podfile文件应该和你的工程文件.xcodeproj在同一个目录下
vim Podfile(先cd到项目所在文件夹)
```
将依赖的库名字依次列在文件中
```
platform :iospod 'JSONKit', '~> 1.4'pod'Reachability', '~>3.0.0' 
pod'AFNetworking', '2.0.0' pod 'RegexKitLite'
```
保存并退出 按下esc,然后输入:wq

##更新框架
```
pod update
```
就会自动为你更新框架,会把仓库所有的框架都会更新一遍,这里注意:CocoaPods在执行```pod install```和```pod update```时,会默认先更新一次CocoPods的 spec仓库索引。使用```--no-repo-update```参数 可以禁止其做索引更新操作
```
pod install --no-repo-update
pod update --no-repo-update
```

##重装cocoa pods
```
sudo gem install -n /usr/local/bin cocoa pods
```
文／aSnail（简书作者）
原文链接：http://www.jianshu.com/p/f6328584bb10
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。




