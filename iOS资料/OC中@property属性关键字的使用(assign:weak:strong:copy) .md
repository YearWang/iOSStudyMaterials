# iOSStudyMaterials
一、assign
用于 ‘基本数据类型’、‘枚举’、‘结构体’ 等非OC对象类型

eg：int、bool等



二、 weak
1. 一般应用： UI控件
2. 详细说明：
(1)为什么建议UI控件一般使用weak？首先我们从controller来看，controller是被系统用强指针引用着，所以如果 controller 还存在，里面的子控件也会存在，那么controller 强引用着它的view（从 controller 中它的 view 的属性是 retain 看出来的，retain 就是 MRC 年代的强引用），那么 view 又强引用着它的数组对象subviews，数组对象又引用着它所包含的数组内容，所以当我们创建出来一个UI控件并将其加入到subviews的时候，它就会被一个强指针所引用着，我们可以简化一下这个过程：--> Controller --> View --> Subviews(数组) --> 数组内容(添加到其中的UI控件)

(2)清楚了过程之后，我们来看我们所创建的对象，如果我们所创建的是一个临时变量的话，那么当出去作用域之后对象就被销毁，但是这里请注意，这里分为两个内存空间，一个是对象的内存空间，一个是指针的内存空间，如果创建的是临时变量的话，一旦出了作用域那么我们的指针内存是被清空了，但是我们的内容如果加到了subviews中，就会被subviews强引用，那么我们的控件就还会存在，只不过是一个指向它的指针被清空了而已。

(3)回过头我们说说全局变量，全局变量的话，指针会一直存在，这里面谈谈为什么要用weak，其实只要我们创建的控件加入到subviews中去的话，那么这个控件就会一直存在，所以在这里我们所创建的指针是weak或strong其实只不过是多一个实线虚线的问题，也就是控件已经被强引用了，你再给它添加一个强引用或者弱引用在使用上都不会有什么问题，但是问题来了，如果我们remove了这个控件，我们subViews中的那根线被切断，也就是这个代表我不再需要这个控件了，那么这个时候如果再用一个strong来连接它，那么对象就不会被清除，既然我们都不需要它了，为什么我还强引用它？这也就是为什么我们再这里用弱引用的原因。`简言之，就是内存使用上的合理性，当这个控件我们需要的时候其实已经有一个强引用在引用着它，我们没有必要再弄一根指针来强引用着它，当我们不需要它的时候，如果是weak的话自然而然直接释放掉了，如果strong的话还会保留它，既然我们没用了我们为什么还要留着它而占用我们宝贵的内存呢？我们也可以看一下这张图片用来理解：



(4)这里特殊说一下IBOutlet中的拖线创建，我们可以发现，如果用storyboard或者xib进行的脱线创建，苹果都会自动降属性置为weak，这种做法似乎也符合我们之前的说法，但是苹果又会在之前加了一个IBOutlet这个关键词，那么这个词是什么意思呢？我们看一下苹果的官网解释： The symbol IBOutlet is used only by Xcode, to determine when a property is an outlet; it has no actual value. 意思很清楚了，它仅仅是指定了一个属性是一个 外部设置的，并没有实质的含义，通常与外界连接是通过当前的 viewcontroller 。那么在引用上又有什么不同呢？在官方文档中是这么说的，在我们创建了IBOutlet之后，我们系统会有一个自动对它进行一个强引用，也就是又多了一条实线连接着它，当控件从我们的subviews中移除之后，这条线会自动判断去留，也就是不会对我们的内存的性能造成影响，该在的时候我在，该消失的时候自己就会消失了。这里也用一张图来说明：





在我写demo的时候有一点需要注意，就是当我们将控件从subViews中remove调之后，这个时候打印指针还存在，说明指针并不会马上销毁，而是进行下一轮消息循环之后才会发现这个指针被销毁了.



3. 总结：
我们首先是从内存的利用上，我们建议对UI控件采用weak，其次是观察苹果的声明方式，依然是建议使用weak，因为标准都是参考于苹果，而且合理性也摆在那里，为什么不用呢？ 


三、 strong
OC对象类型（NSArray、NSDate、NSNumber、模型类）

一个对象只要有强指针引用着，就不会被销毁 



四、 copy
1. 一般用在NSString*类型、block类型上

2.  copy语法的作用：产生副本。 且copy返回的是不可变的副本，mutableCopy返回的是可变的副本。

3. 修改了副本并不会影响源对象，修改了源对象，并不会影响副本。

4. copy在属性声明中的使用，直接举例说明

@interface ViewController ()//注意这里虽然是copy的属性，但是我们这个指针还是强引用的

@property(nonatomic,copy)NSString*name;

@end



@implementation Viewcontroller

-（void）viewDidLoad

{ 

[self viewDidLoad]; 

NSMutableString*str = [NSMutableString stringWithFormat:@"aaa"]; 

self.name = str; 

[str appendString:@"bbb"]; 

NSLog(@"str= %@",str); 

NSLog(@"name = %@",self.name); 

NSLog(@"%p,%p",str, self.name);

}

@end



我们来看一下说出的结果：

str = aaabbb

name = aaa

输出的内存地址：0x7f90f3028180 , 0x7f90f3027450



这说明了这个copy的含义就是，我们在给name属性赋值的时候，，系统默认先将str执行一次copy方法，然后再将结果赋给我们的属性，只有这样你再之后对str修改之后，name的值还是不变的，说明两个指针其实指向的是不同的内容，之后我们又打印了我们的指针值，得出不同的结果又证明了上述所说。



5. 注意：并不是所有情况下我们的string都必须使用copy，因为如果我们的需求是希望string是随着我的改变而改变的，那么这个时候应该使用strong。



6. 类的copy：

如果我们想实现类的copy，必须实现一个方法：-(id)copyWithZome:(NSZone*)zone; 这是为什么呢？我们去 NSString 中去寻找答案，那么我们会发现其实 NSString 已经遵守了 NSCopying 与 NSMutableCopying 的协议，我们主要看 NSCopying ，进入这个协议之后你会发现 -(id)copyWithZome:(NSZone*)zone 这个方法，也就是说NSString 已经遵守了协议的这个方法，所以才能直接实现 copy 的方法。所以如果想实现自定义类的 copy 方法，我们是需要先遵守 NSCopying 协议，然后实现-(id)copyWithZome:(NSZone*)zone的方法:

-(id)copyWithZone:(NSZone *)zone

{

Mitchell*copyMit = [[Mitchell allocWithZone:zone] init];

copyMit.name = self.name;return copyMit;

}

zone：系统返回给我们 copy 对象的内存空间

注意：必须在初始化方法中给属性赋值，才能让 copy 出的对象和原来的对象有相同的属性。



7. 再说一下 copy 类中的 set 方法，如果属性是 copy 的，那么系统默认只会在 set 方法中调用 copy 的方法：

-(void)setName:(Mitchell*)name

{ 

_name = [name copy];

}

