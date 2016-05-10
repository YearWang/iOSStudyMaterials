# 字符串NSString与NSMutableString

---

字符串算是OC中非常重要和常用的一部分内容，OC中的字符串与我之前在学习C,C++,Java中的字符串有一定的不同，它非常类似于C++中容器的概念，但用法却与之还是有很大的不同，也许是因为OC的语法就与其他我们常用的编程语言不尽相同。

这里总结一下字符串NSString与NSMutableString。

##一. NSString

NSString代表字符序列不可变的字符串，NSString的功能非常强大，OC的字符串处理比C语言的饿字符串简单、易用得多。

这里我们通过一个具体的例子来进行分析。

**创建两个字符串对象：**
```
NSString *str1 = @"this is string A";
NSString *str2 = @"this is string B";
```
**计算字符串中的字符个数：**
```
NSLog(@"Length of str1 : %lu" , [str1 length]);
```
**利用stringWithString 将一个字符串复制到另一个字符串：**

```
res = [NSString stringWithString : str1];
NSLog(@"copy : %@" , res);
```
**stringByAppendingString，将一个字符串复制到另一个字符串的末尾：**
```
str2 = [str1 stringByAppendingString:str2];
```
**isEqualToNumber :方法比较两个NSNumber对象的数值。程序会返回一个BOOL值，查看这两个值是否相等。**

**isEqualToString，判断两个字符串是否相等：**
```
if([str1 isEqualToString: res] == YES)
            NSLog(@"str1 == res");
else
            NSLog(@"str1 != res");
```
**compare : 方法测试一个值是否在数值上小于、等于或大于另一个值。
如： [intNumber compare : myNumber]
若intNumber 小于 myNumber ，返回NSOrderedAscending ;
相等                    ，返回NSOrderdSame;
               大于                    ，返回NSOrderdDescending**
```
       //验证一个字符串是否小于、等于或大于另一个字符串
        compareResult = [str1 compare: str2];
        
        if(compareResult == NSOrderedAscending)
            NSLog(@"str1 < str2");
        else if(compareResult == NSOrderedSame)
            NSLog(@"str1 == str2");
        else
            NSLog(@"str1 > str2");
```
**uppercaseString,将字符串转换为大写。
lowercaseString,将字符串转换为小写。**
```
        //将字符串转换为大写
        res = [str1 uppercaseString];
        
        //将字符串转换为小写
        res = [str1 lowercaseString];
```

**stringByAppendingString,在字符串后面添加固定的字符串：**
```
str = [str stringByAppendingString:@", iOS!"];
```
**substringToIndex，获取str的前10个字符组成的字符串：**

**substringToIndex：方法创建了一个子字符串，包括首字符都指定的索引数，但不包括这个字符**。因为索引数是从0开始的，所以参数3表示从字符串中提取0、1、2，并返回结果字符串对象。对于所有采用索引数作为参数的字符串方法，如果提供的索引数对该字符串无效，就会获得```Range or index out of bounds```的出错信息。
```
        //获取str的前10个字符组成的字符串
        NSString *s1 = [str substringToIndex:10];
        NSLog(@"%@" , s1);
```
**substringFromIndex，获取str从第5个字符开始，与后面字符组成的字符串：**
```
        //获取str从第5个字符开始，与后面字符组成的字符串
        NSString *s2 = [str substringFromIndex:5];
        NSLog(@"%@" , s2);
```
**获取str从第5个字符开始，到第15个字符组成的字符串：**
```
//获取str从第5个字符开始，到第15个字符组成的字符串
NSString *s3 = [str substringWithRange:NSMakeRange(5, 15)];
NSLog(@"%@" , s3);
```
**rangeOfString ， 获取ios在str中出现的位置：**
```
//获取iOS在str中出现的位置
NSRange pos = [str rangeOfString:@"iOS"];
NSLog(@"ios在str中出现的开始位置：%ld , 长度为：%ld" , pos.location , pos.length);
```

##二. NSMutableString

NSMutableString对象代表一个字符序列可变的字符串，而且NSMutableString是NSString的子类，因此前面介绍的NSString所包含的方法，NSMutableString都可以直接使用，NSMutableString对象也可直接当成NSString对象使用。

**stringWithString，用不可变字符串创建可变字符串：**
```
        //从不可变字符串创建可变字符串
        mstr = [NSMutableString stringWithString:str1];
        NSLog(@"%@" , mstr);
```
**insertString，插入字符：**
```
        //插入字符
        [mstr insertString:@"mutable" atIndex:7];
        NSLog(@"%@" , mstr);
```
**insertString:  atIndex:   ,插入末尾进行有效拼接：**
```
//插入末尾进行有效拼接
[mstr insertString:@" and string B" atIndex:[mstr length]];
NSLog(@"%@" , mstr);
```
**deleteCharactersInRange:NSMakeRange() , 根据范围删除子字符串：**
```
        //根据范围删除子字符串
        [mstr deleteCharactersInRange: NSMakeRange(16, 13)];
        NSLog(@"%@" , mstr);
```
**查找然后直接删除：**
```
//查找然后将其删除
substr = [mstr rangeOfString:@"string B and "];
        
if(substr.location != NSNotFound){
         [mstr deleteCharactersInRange:substr];
         SLog(@"%@" , mstr);
        }
```
**替换一些字符**
```
[mstr replaceCharactersInRange:NSMakeRange(8, 6) withString:@" a mutable string"];
NSLog(@"%@" , mstr);
```
```
 1 #import <Foundation/Foundation.h>
 2 
 3 int main(int argc, const char * argv[])
 4 {
 5 
 6     @autoreleasepool {
 7         
 8         // insert code here...
 9         //NSLog(@"Hello, World!");
10         
11         NSString *str1 = @"this is string A";
12         NSString *search , *replace;
13         NSMutableString *mstr;
14         NSRange substr;
15         
16         //从不可变字符串创建可变字符串
17         mstr = [NSMutableString stringWithString:str1];
18         NSLog(@"%@" , mstr);
19         
20         //插入字符
21         [mstr insertString:@"mutable" atIndex:7];
22         NSLog(@"%@" , mstr);
23         
24         //插入末尾进行有效拼接
25         [mstr insertString:@" and string B" atIndex:[mstr length]];
26         NSLog(@"%@" , mstr);
27 
28         
29         //直接使用appendString
30         [mstr appendString:@" and string C"];
31         NSLog(@"%@" , mstr);
32         
33         //根据范围删除子字符串
34         [mstr deleteCharactersInRange: NSMakeRange(16, 13)];
35         NSLog(@"%@" , mstr);
36         
37         //查找然后将其删除
38         substr = [mstr rangeOfString:@"string B and "];
39         
40         if(substr.location != NSNotFound){
41             [mstr deleteCharactersInRange:substr];
42             NSLog(@"%@" , mstr);
43         }
44         
45         
46         //直接设置为可变的字符串
47         [mstr setString:@"this is string A"];
48         NSLog(@"%@" , mstr);
49         
50         //替换一些字符
51         [mstr replaceCharactersInRange:NSMakeRange(8, 6) withString:@" a mutable string"];
52         NSLog(@"%@" , mstr);
53         
54         //查找和替换
55         search = @"this is";
56         replace = @"an example of";
57         
58         substr = [mstr rangeOfString:search];
59         
60         if(substr.location != NSNotFound){
61             [mstr replaceCharactersInRange:substr withString:replace];
62             NSLog(@"%@" , mstr);
63         }
64         
65         //查找和替换所有的匹配项
66         search = @"a";
67         replace = @"X";
68         
69         substr = [mstr rangeOfString:search];
70         
71         while (substr.location != NSNotFound) {
72             [mstr replaceCharactersInRange:substr withString:replace];
73             
74         }
75         
76         NSLog(@"%@" , mstr);
77         
78     }
79     return 0;
80 }
```



