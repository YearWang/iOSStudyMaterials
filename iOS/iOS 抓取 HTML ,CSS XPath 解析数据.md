# iOS 抓取 HTML ,CSS XPath 解析数据


---

以前我们获取数据的方式都是使用 AFN 来 Get JSON 数据,比如 点我查看 JSON 数据.

但例如下面的百度贴吧,和豆瓣读书等网站..并不提供我们获取数据的 API

百度贴吧:
![百度贴吧](http://bigpi.me/ios/_image/iOS%E6%8A%93%E5%8F%96HTMLCSSXPath%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE/%E7%99%BE%E5%BA%A6%E8%B4%B4%E5%90%A7%E6%95%B0%E6%8D%AE.png)

豆瓣读书:
![豆瓣读书](http://bigpi.me/ios/_image/iOS%E6%8A%93%E5%8F%96HTMLCSSXPath%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE/%E8%B1%86%E7%93%A3%E8%AF%BB%E4%B9%A6%E6%95%B0%E6%8D%AE.png)

这时我们可以解析他们的 HTML 来获取我们想要的数据.

##工具准备

这时我们需要2个工具,`Firefox` 和`FireBug`.

你可以在 这里下载 FireFox 浏览器,然后在其右上角菜单的附加组件管理器中下载 FireBug 插件.
FireBug 有很强大的 JavaScript 调试功能,还能实时编辑 HTML CSS,是前端同学喜爱的一个工具.
下载安装好以后 点击右上角的 Bug(虫子)图标来使用 FireBug 调试当前网页.

如果你不了解 XPath ,可以学习 w3school 的教程.
![](http://bigpi.me/ios/_image/iOS%E6%8A%93%E5%8F%96HTMLCSSXPath%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE/%E6%89%93%E5%BC%80%20FireBug.png)
##Ono 开源库

Ono 是一个 Github 上的开源项目,它能方便我们解析 XML,HTML 标签,并且支持 CSS XPath 搜索特定节点.

你可能没听过这个库,但其作者你肯定知道. Mattt Thompson,它是 AFN 的作者,还是博客 NSHipster 的作者.

Swift 版本类似的开源库 Ji

Java 或 Android 可以使用 Jsoup

##开始

准备工作都 Ok 了..我们开始编码.新建一个空白工程,注意,如果要在 `Info.plist` 中添加两行 `App Transport Security Settings`,和 `Allow Arbitrary Loads` `YES`, 来允许 HTTP 传输.
![](http://bigpi.me/ios/_image/iOS%E6%8A%93%E5%8F%96HTMLCSSXPath%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE/App%20%E5%85%81%E8%AE%B8%20Http.png)

然后使用 CocoaPods 添加第三方库 `pod 'Ono'`.

这里,要解析的 HTMl 数据就用我的博客了

再创建一个 Post 类继承自 NSObject,代表每一篇文章 ,修改 `.h` 文件如下
```objective-c
#import <Foundation/Foundation.h>
@class ONOXMLElement;

@interface Post : NSObject
@property (copy,nonatomic) NSString *title; //文章标题
@property (copy,nonatomic) NSString *postDate; //文章发表时间
@property (copy,nonatomic) NSString *postUrl; //文章正文内容的 Url

+(NSArray*)getNewPosts; //获取所有文章
+(instancetype)postWithHtmlStr:(ONOXMLElement*)element; //用 HTMl 数据创建 Post 类
@end
```
在`.m `文件中导入 Ono,并添加一个常量 Url.
```
#import <Ono.h> 

static NSString *const kUrlStr=@"http://BigPi.me";
```
然后我们可以用 AFN 等下载该 Url 的 HTML数据,再使用 XPath 获取代表每一篇文章的 XPath,

先打开 FireFox 和 FireBug ,点击下面的图标
![](http://bigpi.me/ios/_image/iOS%E6%8A%93%E5%8F%96HTMLCSSXPath%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE/FireBug%20%E5%85%83%E7%B4%A0%E9%80%89%E6%8B%A9%E5%99%A8.png)

在适当移动鼠标,点击选择网页上的一篇文章

![](http://bigpi.me/ios/_image/iOS%E6%8A%93%E5%8F%96HTMLCSSXPath%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE/Post%E6%95%B0%E6%8D%AE.png)

这时我们可以看到,下面 FireBug 的 HTMl 树展开了,我们可以发现,每一个 `<div class="post">`标签都包含一篇文章的数据.

我们右键 `<div id="posts"> `,选择`复制最简XPath`
![](http://bigpi.me/ios/_image/iOS%E6%8A%93%E5%8F%96HTMLCSSXPath%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE/%E5%A4%8D%E5%88%B6%20XPath.png)

复制出来的结果` //*[@id="posts"]`,这个` <div id="posts">`节点下面的每一个子节点都代表一篇文章,

我们现在来使用这个 XPath 获取所有的 HTML 数据.

在` Post.m` 添加如下方法:
```objective-c
+(NSArray*)getNewPosts{
    NSMutableArray *array=[NSMutableArray array];
    NSData *data= [NSData dataWithContentsOfURL:[NSURL URLWithString:kUrlStr]]; //下载网页数据

    NSError *error;
    ONOXMLDocument *doc=[ONOXMLDocument HTMLDocumentWithData:data error:&error]; 
    ONOXMLElement *postsParentElement= [doc firstChildWithXPath:@"//*[@id='posts']"]; //寻找该 XPath 代表的 HTML 节点,
    //遍历其子节点,
    [postsParentElement.children enumerateObjectsUsingBlock:^(ONOXMLElement *element, NSUInteger idx, BOOL * _Nonnull stop) {
        NSLog(@"%@",element);
    }];
    return array;
}
```
并在ViewController.m 中调用这个方法:
```
@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    [Post getNewPosts];
}

@end
```
运行后查看 Console, 我们已经可以获取到每篇文章的 HTMl 了,然后我们再来解析每篇文章的具体数据.

切换到 FireBug,展开其中一篇文章的节点,

![ ][1]

我们可以看到`<h2 class="title">`节点下的

`<a href="/post/jazzhands/jazzhands-yuan-ma-shi-xian-fen-xi">`
`<i class="fa fa-leaf"></i>`
`JazzHands 源码实现分析</a>`

标签中,有文章的具体Url, 和文章标题,

`<div class="info">`节点下的:

`<span class="date">`
`<i class="fa fa-clock-o"></i>`
`2016-03-04 21:39</span>`

标签有文章发布的时间,此时我们可以右键点击节点,复制文章标题,发布时间等节点的 XPath,但这里我们使用相对的 XPath.

每篇文章的 HTML 结构如下:

`<div class="post">`
`文章标题 Url等内容`
`</div>`

所以我们的

    文章 Url XPath : "h2/a"
    文章标题 XPath : a 标签的 href 属性值
    文章发布时间 XPath : "div[2]/span[1]"

接下来我们来解析每一篇文章的详细数据

在 `Post.m` 中添加方法:
```
+(instancetype)postWithHtmlStr:(ONOXMLElement*)element{

    Post *p=[Post new];
    ONOXMLElement *titleElement= [element firstChildWithXPath:@"h2/a"]; // 根据 XPath 获取含有文章标题的 a 标签
    p.postUrl= [titleElement valueForAttribute:@"href"]; //获取 a 标签的  href 属性
    p.title= [titleElement stringValue];
    ONOXMLElement *dateElement= [element firstChildWithXPath:@"div[2]/span[1]"]; //根据 XPath 获取文章发布时间 span 标签
    p.postDate= [dateElement stringValue];
    return p;
}
```
然后修改 `+(NSArray*)getNewPosts`方法:如下
```
...
[postsParentElement.children enumerateObjectsUsingBlock:^(ONOXMLElement *element, NSUInteger idx, BOOL * _Nonnull stop) {
        //NSLog(@"%@",element);
        Post *post=[Post postWithHtmlStr:element];
        if(post){
            [array addObject:post];
        }
    }];
...
```
最后因为我们我们获取到的 HTMl 的文章 Url 是相对 Url, 类似 
`/post/jazzhands/jazzhands-yuan-ma-shi-xian-fen-xi`
所以我们在 Setter 方法中拼接域名 ,` http://BigPi.me`
```
-(void)setPostUrl:(NSString *)postUrl{
    _postUrl=[kUrlStr stringByAppendingString:postUrl];
}
```
我们在下图位置打断点查看结果:

![ ][1]


  [1]: http://bigpi.me/ios/_image/iOS%E6%8A%93%E5%8F%96HTMLCSSXPath%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE/%E4%BB%A3%E7%A0%81%E6%96%AD%E7%82%B9.png
  
  运行起来,结果如下:
  
  ![](http://bigpi.me/ios/_image/iOS%E6%8A%93%E5%8F%96HTMLCSSXPath%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE/%E6%8A%93%E5%8F%96%E6%96%87%E7%AB%A0%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%9C.png)
  
  至此我们已经能使用 FireBug + Ono + XPath 来解析 HTML 数据

**我就使用这个办法获取我们学校教务管理系统HTML,制作了一个统计成绩,计算绩点的 App.**

##补充

FireBug 是一个很强大前端调试工具.

还可以使用 正则表达式 来解析 HTML 数据.不过从 StackOverflow 讨论 来看 ,并不推荐使用正则来解析 HTML 数据.

RayWonderLich 有一篇比较老的教程 ,使用类似的技术解析 HTML

最最后,很重要的一点,HTML 数据可能经常会变动,尤其那个网页还不是我们自己能管理的网页,所以 XPath 随时可能解析失败,

如果你一定要使用 XPath 来解析 HTML 数据,可以在服务端进行这个操作,然后修改成 API 的形式,让手机端像以前一样 GET JSON 数据.

同时,服务端还可以设置异常处理,缓存等策略.

本文 Demo 可以在 [GitHub](https://github.com/Big-Pi/ParseHTMLDemo) 中获取