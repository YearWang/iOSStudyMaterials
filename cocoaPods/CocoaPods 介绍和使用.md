# CocoaPods 介绍和使用


---

##简介
**1.什么是CocoaPods？**
CocoaPods 是开发 OS X 和 iOS 应用程序的一个第三方库的依赖管理工具。利用 CocoaPods，可以定义自己的依赖关系 (称作 pods)。CocoaPods是用 Ruby 写的，并由若干个 Ruby 包 (gems) 构成的。

**2.为什么使用CocoaPods？**
首先，在工程中引入第三方代码时，CocoaPods简化了配置 build phases 和 linker flags ，它能够自动配置编译选项。其次，通过 CocoaPods，可以很方便的查找到新的第三方库。以此来缩短我们的开发周期和提升软件的质量。

##安装
**1.升级gem**

获得超级管理员权限更新gem，需要输入密码

> sudo gem update --system

**2.替换Ruby源**

移除Ruby官网的软件源，添加成淘宝的软件源 

> gem sources --remove https://rubygems.org/ 
> gem sources --add https://ruby.taobao.org/

查看当前Ruby软件源

> gem sources -l

提醒：ruby.taobao.org是一个完整 rubygems.org 镜像，你可以用此代替官
方版本，同步频率目前为15分钟一次以保证尽量与官方服务同步

**3.安装CocoaPods**

用超级管理员权限安装CocoaPods

> sudo gem install cocoapods

**4.初始化CocoaPods**

> 移除主支 
> pod repo remove master 
> 添加新的主支 
> pod repo add master https://gitcafe.com/akuandev/Specs.git 
> 更新 
> pod repo update 
> 初始化 
> pod setup

文／一枝枫叶红深秋（简书作者）
原文链接：http://www.jianshu.com/p/200365685a96
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。




