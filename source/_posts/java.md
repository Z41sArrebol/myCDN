---
title: Java基础
author: Z41sArrebol
date: 2025-02-23 11:23:03
tags:
    - Java
cover: /img/girls/月下巫师.jpeg
categories: 编程...？
---



# 变量

1.变量等同于一个有命名的存储地址

2.变量是解决内容复用的

3.变量是有名称的，变量内容就是对象

## 声明(declaration)

例 ：

```java
string string;  
int age;
```

注意：关键字无法进行声明，如public，class，void，int

## 赋值

例：

```java
String name = "小明"  
int age = 17
```

## 数据类型

1.数据基本类型：字符串，数字，布尔。

2.数字类型包含 ：

int小整数（-2的31次方~2的31次方-1）

long大整数（-2的63次方~2的63次方-1）

double浮点数

## 运算符

加减乘除：+ - * /  取模：%

注意：Java执行的是整数除法，当两个数都是整数时，结果也自动变为正数。

# 无返回值方法

## 数学函数

函数在Java中也称为方法

随机数方法的应用较为广泛

```java
double value = Math.random();
System.out.println(value);
```

返回随机数的范围是：0.0<= **Math.random** <1.0 浮点类型的数据

如果需要得到0-9的随机数，那就需要进行类型转换+乘10操作

类型转换的语法为：

​`int nValue = (int) value;`​

```java
// (value*10) 用小括号包围是因为先乘10得到结果在转化为整数
// 要不然得不到结果，这个就是数学的优先级
int nValue = (int) (value*10);
```

## 自定义方法

方法的语法为：

```java
// public（公共的） static（静态） void（空类型）
public static void 方法名称(方法参数){
  代码块
}
```

### 驼峰式命名法

大驼峰 例：UserName MyDocument

小驼峰 例：userName passWd

方法的命名遵循小驼峰原则

方法的调用：直接写出即可，如new方法调用时写出 new();

### 形参与实参

在方法中声明的变量成为形参，而在方法中传递的变量成为实参

在调用方法时，要传递进去一个“实际的”变量，这个变量就叫做实参。

# 条件执行

### if，else与else if

格式： if(表达式){   代码块  }

接上的else语句没有( )部分，其他同理

如果使用else if，则与if同理

```java
public static void report(String name,double score){ 
  if(score >= 90){
    System.out.println(name+",你本次的成绩为优秀");
  } else if(score >= 80){
    System.out.println(name+",你本次的成绩为良好");
  } else if(score >= 60){
    System.out.println(name+",你本次的成绩为及格");
  } else {
    System.out.println(name+",你本次的成绩为不通过");
  } 
}
```

### 嵌套条件

即 if 里面继续套 if / else语句

### 返回语句

```java
public static void plan(double temperature){

  System.out.println("准备出门去学校");

  if(temperature>38){
    // 体温超过38度，不去了
    return;
  }
  System.out.println("去操场踢球");
}
```

如果碰到体温大于38度的情况，就会"return" ，结束后续代码的执行。

注意：return后面不能有其它代码，除非在嵌套代码中（如上面的例子）

### 递归

即一个方法也可以调用自身。

例：

```java
public class CountDown {

  public static void main(String[] args) {
    count(3);
  }

  public static void count(int number) {
    System.out.println(number);
    number = number - 1;

    if (number == 0) {
      System.out.println("程序执行完毕");
      return;
    }
    count(number);
  }
}
```

# 有返回值方法

## 返回值

void：无返回值  其他类型：有返回值

```java
public static void code(int len)
public static double random()
//返回一个double类型的随机数
```

声明double之后必须执行return返回结果

返回结果的类型必须与方法声明的返回值一致

## 布尔&&逻辑运算

true与false

```java
boolean flag = x>0;
if(flag){
  	System.out.println("x>0");
}
```

boolean类型也可以作为方法的参数或者返回类型

```java
public static boolean isPass(int score){
  	return score >= 60;
}
```

### 逻辑运算符：

and（与）：&&

or（或者）：||

not（非）：！

```java
if(x>0 && x <10){
  // 表示 x 大于0 并且 x小于10时才为真
}
if(x == 60 || x == 80){
  // 表示 x 等于60 或者 x 等于80 都为真
}
if(x!=0){
  // 表示x不等于0的时候
}
```

# 循环

## 自增与自减

自增同理

```java
number = number - 1;
//等同于
number--;
//等同于
number -= 1;
```

## 数组

变量创建方式与普通变量相似，但是后面跟了个 [ ]

初始化规定了数组的大小

如果没有存储实际的值，int类型的数据默认就是0，String类型默认是null

```java
// 声明一个 int 数组的变量，数组大小为6
int[] numbers = new int[6];
```

赋值可以借助下标来进行操作

0位代表第一个数据

```java
numbers[0] = 1;
```

### 相关方法

数组的长度.length

```java
public static void main(String[] args) {
  int[] numbers = new int[8];
  int size = numbers.length;
  System.out.println(size);
}
```

## for循环

```java
String[] tables = new String[3];
tables[0] = "张三";
tables[1] = "李四";
tables[2] = "王五";
for(int i = 0; i< tables.length;i++){
  String name = tables[i];
  System.out.println(name);
}
```

int i=0 声明了一个新的计数器，从零开始工作  
i < tables.length 声明了计数器的条件，最大值不超过tables.length  
i++ 声明了计数器的递增机制，这里是递增，每次+1

# 字符串

## 相关方法

### length长度

```java
public static void main(String[] args) {
  String message = "我在学习";
  // 调用字符串的长度方法得到长度
  int size = message.length();
  System.out.println(size);
}
```

### charAt单字符

```java
public static void main(String[] args) {
  String message = "Hello Java";
  // 取出第一个字
  char str = message.charAt(0);
  System.out.println(str);
  // 取出第二个字
  str = message.charAt(1);
  System.out.println(str);
}
```

注意：chatAt方法返回的数据类型是char，单个字符的类型就是char，只能存储一个字符

### trim去空格

```java
public class Test {
  public static void main(String[] args) {
    String str = " 666 ";
    // 去空格
    String newStr = str.trim();
    // 打印一下新字符串的length，对比老的字符串的length
    System.out.println(newStr+"的长度是:"+newStr.length());
    System.out.println(str+"的长度是:"+str.length());
  }
}
```

### indexOf查找

indexOf方法接受一个String字符串，调用时，取文本中查找第一个匹配到的坐标索引值。

所以执行后可以得到一个int数字类型的数组，通过int数字的值来判断是否匹配

如果返回值是-1说明不匹配，如果不是-1，那就匹配到了

```java
 public static void main(String[] args) {
    String str = "Java是一种广泛使用的计算机编程语言"
    int index = str.indexOf("Java");
    if(index!=-1){
      System.out.println("匹配到了Java，索引位置是"+index);
    }else{
      System.out.println("没有匹配到Java");
    }
  }
```

如果想匹配第二个Java，可以设置从第一个Java的位置来查找

```java
 int index1 = str.indexOf("Java");
 index1 = str.indexOf("Java", index + 4);
// 这里4是开始查找的索引值，可以替换成string.length来计算
```

### substring拼接

调用方式：

substring（开始索引，结束索引）如果没有设置结束索引，那就是默认到字符串结束

```java
 public static void main(String[] args) {
    String str = "Java是一种广泛使用的计算机编程语言"
    int index = str.indexOf("Java");
    if (index != -1) {
      System.out.println("匹配到了Java，索引位置是" + index);
      String newStr = str.substring(index+4);
      System.out.println(newStr);
    } else {
      System.out.println("没有匹配到了Java");
    }
  }
```

### startsWith/endsWith内容判断

endsWith方法接受一个字符串参数，用于判断是否以该字符串结束，返回类型是boolean类型

```java
  public static void main(String[] args) {
    String fileName = "报告.doc";
    if(fileName.endsWith(".doc")){
      System.out.println("是word文档");
    }
  }
```

### replaceAll替换

​`replaceAll("要替换的值","新值")`​

执行后会得到一个新的字符串，原始的字符串并不发生改变

```java
 public static void main(String[] args) {
    String str = "Java是一种广泛使用的计算机编程语言"
    String newStr = str.replaceAll("Java","Python");
    System.out.println(newStr);
    System.out.println(str);
  }
```

删除内容也可以采用这个方法，把替换值换成空格即可

### split分割

```java
public static void main(String[] args){
  String text = "姓名|年龄|性别\n张三|20|男\n李四|18|男\n王五|18|男";
  // 使用 split 进行换行符的分割，得到一个新的数组对象
  String[] data = text.split("\n");
  // 因为第一行是标题不是数据，所以我们需要把长度-1
  // (注意要使用小括号包围，因为要先计算长度再组合字符串)
  System.out.println("共有:"+(data.length-1)+" 条记录");
	for(int i=1;i<data.length;i++){
    // 使用 \\| 进行字符串分割，得到一个新数组对象
    	String[] lines = data[i].split("\\|");
    	System.out.println("姓名:"+lines[0]+" 年纪:"+lines[1]+" 性别:"+lines[2]);
  }
}
```

​`注意：. | *这三个字符如果作为分割符，就需要加上\\，比如str.split("\\|")`​

### toUpperCase/~Lower大小写转化

```java
public static void main(String[] args) {
    String text = "ZhanSan";
    // 把拼音全部转化为大写字母
    String enName = text.toUpperCase();
    System.out.println(enName);
  }
```

### equals比较

```java
public static void main(String[] args) {
    String text = "字符串";
    // 使用 equals 方法判断是否相同
    if (text.equals("字符串")) {
      System.out.println("equals 方法字符串相等");
    }
    // 前后顺序无所谓,下面代码是一样的
    if ("字符串".equals(text)) {
      System.out.println("equals 方法字符串相等");
    }
  }
```

### 数字字符串转化Integer.parseInt

```java
 public static void main(String[] args) {
    String text = "123";
    // 转化字符串为数字
    int a = Integer.parseInt(text);
    System.out.println(a);
  }
```

数字转化为字符串有两种方法，一种是之前运用的+，比如

```java
 public static void main(String[] args) {
    int a = 100;
    //使用空字符串相加数字，会自动变成字符串类型
    String str = ""+a;
    System.out.println(str);
  }
```

另一种是利用`String.valueOf( )`​方法，该方法参数接受数字，浮点，布尔类型转化为字符串

```java
 public static void main(String[] args) {
    int a = 100;
    //使用valueOf强制把数字转化为字符串
    String str = String.valueOf(a);
    System.out.println(str);
  }
```

‍

‍

‍
