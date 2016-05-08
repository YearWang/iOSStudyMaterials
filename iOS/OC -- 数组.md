# OC -- 数组


---

OC中数组的格式：NSSArray *array = @[@"元素1",@"元素2",@"元素3"];

OC中能将另一个数组的值赋给新数组

##1.  数组对象的创建 

(1).  创建数组时给数组添加一个元素：arrayWithOb

(2). 创建数组时给数组添加多个元素：arrayWithObjects 

      采用该方式最后用 nil结尾

(3). 创建数组时添加整个数组：arrayWithArray

               NSArray *array = @[@"蝙蝠侠",@"钢铁侠",@"煎饼侠"];

                NSArray *array1 = [[NSArray alloc]init];

              array1 = @[@"葫芦娃",@"女娲",@"孙悟空"];
              

//oc中能够将另一个数组的值赋给新数组

                NSArray *array2 = [NSArray array];


               array2 = array1;

 //ArrayWithObject,创建数组时只能给数组赋一个值

                NSArray *array3 = [NSArray arrayWithObject:@"sss"];

//ArrayWithObjects,创建数组是能够给数组赋多个元素

                NSArray *array4 = [NSArray arrayWithObjects:@"sb",@"nc", nil];


//arrayWithArray,用已有数组创建新数组
**NSArray *array5 = [NSArray arrayWithArray:array];**

##2.  获取数组中的元素个数以及访问数组元素

**(1). 通过下标来存取值：array[ ];**

NSArray *array8 = [NSArray arrayWithObjects:@"大天儿",@"中天儿",@"小天儿", nil];
NSString *test = array8[2];//OC数组通过下标来取值

**(2). 通过count获取到数组中元素的个数：array count**

 int Numelement = [array8 count];

##3.  追加数组中的内容

**(1). 往数组里面追加一个元素：arrayByAddingObject**

NSArray *arrayAdd = [array8 arrayByAddingObject:@(8)];

**(2). 往数组里面追加一个数组：arrayByAddingObjectsFromArray**

 NSArray *array9 = @[@"大娃",@"小娃",@"江娃"];
NSArray *arrayAddArray =[array8 arrayByAddingObjectsFromArray:array9];

##4.  数组转字符串
**(1).数组转换字符串的方法：componentsJoinedByString**

用符号隔开元素

NSArray *array10 = @[@"大咕噜",@"中咕噜",@"小咕噜"];

NSString *arrayString = [array10 componentsJoinedByString:@" "];
 
##5.  判断数组中是否存在一个指定的对象
**(1). 判断数组中是否存在一个指定的对象：containsObject，结果用 BOOL 接受**

BOOL isMieShaoNv = [array10 containsObject:@"美少女"];

##6. 根据指定的对象返回索引下标、返回数组中最后一个元素

**(1).  根据索引下标，找到第一个最后一个元素：**
first/lastObject   NSMutableArray

 **//indexOfObject找到指定对象的下标**

long index = [array10 indexOfObject:@"白素贞"];

**//lastObject获取到数组的最后一个元素**

NSString *lastString = [array10 lastObject];

##7.  新增

**(1). 往数组里面添加一个元素：addObject**

NSMutableArray *array11 = [NSMutableArray arrayWithObjects:@"哪吒",@"喜洋洋",@"光头强", nil];
[array11 addObject:@"鸟山明"];

**(2).往数组里面批量添加元素：addObjectFromArray**

NSArray *array12 = @[@"1",@"2",@"3",];

//addObjectsFromArray 往数组批量添加元素

[array11 addObjectsFromArray:array12]

##8.  插入

**(1). 往数组里插入一个元素：insertObject...atIndex**
**atIndex指的是从哪插入**

 NSArray *array11 = @[@"美美",@"哒",@"哈",];
[array11 insertObject:@"小龙女" atIndex:0];

##9. 删除

**// 移除最后一个元素**

[array11 removeLastObject];

**//移除指定位置的元素**

[array11 removeObjectAtIndex:0];

**//移除数组中指定的元素**

[array11 removeObject:@"路飞"]; 

**//移除所有的元素**

[array11 removeAllObjects];

**//批量移除**

NSArray *Array  = @[@"11",@"22",@"33"];

[array11 removeObjectsInArray:Array ];

##10. 替换元素

**(1). 用指定元素替换数组中指定位置：replaceObjectAtIndex：withObject**
**后加指定元素位置,      后加指定元素**

[array11 replaceObjectAtIndex:0 withObject:@"微笑"];

**(2). 用指定数组替换数组中指定区域元素**

**replaceObjectInRange：位置
withObjectsFromArray：nil**

NSRange rang2 = {1,4};
[array11 replaceObjectsInRange:rang2 withObjectsFromArray:nil];

文／纪銨骊（简书作者）
原文链接：http://www.jianshu.com/p/cbdabec42488
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。




