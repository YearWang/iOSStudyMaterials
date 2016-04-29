# block 中使用__weak 和__strong修饰符?

---
###问题阐述

在ARC环境下，我们常常会**使用weak 的修饰符来修饰一个变量，防止其在block中被循环引用，但是有些特殊情况下，我们在block中又使用strong 来修饰这个在block外刚刚用__weak修饰的变量**，为什么会有这样奇怪的写法呢？

后来上网查资料，给的解释就是下面的这段话：

    在block中调用self会引起循环引用，但是在block中需要对weakSelf进行strong,保证代码在执行到block中，self不会被释放，当block执行完后，会自动释放该strongSelf。
对于程序员来说，文字说明要有，编码就更少不了了；下面是我对上面的话转译成的代码；

**第一步：我们自定义一个类，在该类dealloc方法中加一行打印语句；**
```Objective-C
@interface SampleObject :NSObject
@end

@implementation SampleObject
- (void)dealloc
{
NSLog(@"dealloc %@",[self class]); 
}
@end
```
**第二步：实例化该类，并在block中调用它；（没有加strong修饰符，三秒后释放该对象）**
```objective-c
SampleObject* sample = [[SampleObject alloc]init];

self->sample= sample;

__weakSampleObject* weaksample = self->sample;

dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0), ^{

NSIntegercount =0;

//__strong SampleObject* strongsample = weaksample;

while(count<10) {

count++;

NSLog(@"aaa %@",weaksample);

sleep(1);

}

});

dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(3*NSEC_PER_SEC)),dispatch_get_main_queue(), ^{

self->sample=nil;

});
```
打印结果如下(没有用strong修饰符的打印结果如下)：

![打印结果1](http://upload-images.jianshu.io/upload_images/839134-c649eaa71c418a69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**结论是：如果仅仅使用__weak去修饰变量，当别处把变量释放后，block中该变量也会被释放掉**

那么好，我们在把第二步中的方法修改一下，加上strong修饰符：
```objective-c
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0), ^{

__strongSampleObject* strongsample = weaksample;

NSIntegercount =0;

while(count<10) {

count++;

NSLog(@"aaa %@",strongsample);

sleep(1);

}

});
```
打印结果如下：

![打印结果2](http://upload-images.jianshu.io/upload_images/839134-b8d73d20f6cc9fe8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**结论是当加上修饰符strong时，当别处把变量释放掉，但调用该变量的block如果仍然没有执行结束，那么系统就会等待block执行完成后再释放，对该变量在block中的使用起到了保护作用。当block执行结束后会自动释放掉。**



