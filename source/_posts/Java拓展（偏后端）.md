---
title: Java拓展（偏后端）
author: Z41sArrebol
date: 2025-03-05 15:08:01
tags:
    - Java
cover: /img/girls/窗边少女.jpeg
categories: 编程...？
---

# Vaadin

是一个优秀的web组件框架，可以使用Java后端语言编写前端页面

## TextField

完整类路径：`com.vaadin.flow.component.textfield.TextField`​

实例化：`TextField field = new TextField();`​

可以写上import+类路径，也可以在Maven里选择自动添加

以下是拓展用法：

```java
//文本框的名字
setLabel(String label)
//文本框填充的信息
setPlaceholder(String placeholder)
setValue(String value)
```

效果如下：

​![image](Java后端/image-20250220162133-tnej10g.png)​

## 水平与垂直布局

垂直：`com.vaadin.flow.component.orderedlayout.VerticalLayout`​

水平：`com.vaadin.flow.component.orderedlayout.HorizontalLayout`​

只需记住垂直布局是v开头，水平是h开头即可

## 继承

```java
package org.vaadin.marcus.spring;
import com.vaadin.flow.component.orderedlayout.VerticalLayout;
// Todo 继承于 VerticalLayout 
public class Todo extends VerticalLayout {  }
```

通过继承，ToDo类就具备了VerticalLayout这个垂直布局容器的特性

## add方法

add方法可以添加任意个子组件，如：

```java
TextField field = new TextField();
add(field); //add变量名称
//可同时添加多个组件
add(field,button,good);
```

## Button按钮

完整类路径：`com.vaadin.flow.component.button.Button`​

```java
// 创建一个保存按钮对象
Button addButton = new Button();
// 在当前布局里添加按钮实例
add(addButton);
//给按钮设置文本
addButton.setText("单击此处");
//(或者直接在创建的时候进行赋值)
Button addButton = new Button("单击此处");
```

## Lambda表达式

Lambda表达式也可以成为闭包，允许把函数作为一个方法的参数。

```java
Button loginButton = new Button("Login");
// 给 loginButton 添加一个点击事件
loginButton.addClickListener(click->{
    System.out.println("login");
});
```

更复杂一点，涉及到了一些新的方法getValue

```java
//设置点击事件
loginButton.addClickListener(click -> {
//使用getValue函数获取输入的内容
    String userNameText = userNameField.getValue();
    String pwdText = passwordField.getValue();
//通过加号将变量与字符串进行组合
    String text = userNameText + "/" + pwdText + " login " + " success";
    System.out.println(text);
});
```

再拓展一下equals方法

```java
loginButton.addClickListener(click -> {
    String userNameText = userNameField.getValue();
    if ("admin".equals(userNameText)){
        System.out.println("用户名正确");
   }});
```

新学：userNameText == null以及"".equals(userNameText)

```java
Notification notification = new Notification();
// 只要满足任何一个条件逻辑就成立，使用 || 用来表示或者的关系
//  userNameText == null 用来判断对象是否为 null，这个是java常用的判断条件
// "".equals(userNameText) 用来判断是否为空内容
if(userNameText == null || "".equals(userNameText)){
  notification.setText("need input username");
  notification.open();
  // 程序执行到这就结束，后面的代码不再执行
  return;
} // 如果密码有输入并且密码等于确认密码的值
if (pwdText != null && !"".equals(pwdText) && pwdText.equals(confirmPwdText)) {
  notification.setText("reg success");
} else {
  notification.setText("confirm pwd error");  
}
notification.open();
```

## Notification

如果再点击事件中使用System.out.printIn()，信息便只能在后台看见，如果想要让用户看到的话，就要用到Notidfication组件

几个重要方法如下：

```java
Button loginButton = new Button(text: "Login");
Notification notification = new Notification();
loginButton.addClickListener(click -> {
    String userNameText = userNameField.getValue();
    String pwdText = passwordField.getValue();
    if ("admin".equals(userNameText) && "admin".equals(pwdText)) {
//setText()，用于设置提示的文本
        notification.setText("login success");
//setDuration()，用于设置开启的时间
        notification.setDuration(3000);
//open()，用于开启提示框
        notification.open();
    } else {
        notification.setText("login error");
        notification.setDuration(3000);
        notification.open();
    }});
```

## 自定义方法、注解

```java
// public（公共的） 
public  返回类型 方法名称(方法参数){
  代码块 }
```

注解：可以实现多页面，格式为@Route( )

```java
import com.vaadin.flow.router.Route;
// 斜杠/是默认页面，如果要切换成其他页面，可以更改斜杠里面的内容
@Route("/")
public class Todo extends VerticalLayout {
}
```

‍

# 数据处理

## 封装

在上面完成了用户的数据存储之后，就需要用到封装

把一个真实的用户信息抽象成Java的User对象

**封装事物的类对象，一般会存放在xxx.model的子包里面，一般把封装的对象称为POJO类**

```java
/**
 * @author Z41sArrebol
 * @date 2025/2/22
 */
public class User {
//注意属性private
    private String userName;
    private String password;
//以下内容可以采用ide工具 getter与setter生成
    public String getUserName() {
        return userName;
    }
    public void setUserName(String userName) {
        this.userName = userName;
    }
    public String getPassword() {
        return password;
    }
    public void setPassword(String password) {
        this.password = password;
    }
}
```

## LocalDateTime

在 Java 当中，有两种时间日期类型，一种是 Java8 以前用的 Date，一种是 Java8 以后主推的 LocalDateTime

* ​`java.time.LocalDate`​ 表示日期 比如 2025-02-20，像出生年月日这种的就可以使用
* ​`java.time.LocalTime`​ 表示时间 比如 12:30:00，像几点的会议就可以使用
* ​`java.time.LocalDateTime`​ 表示日期和时间 比如 2025-02-20 12:30:00，像报名时间、注册时间之类的就可以使用

```java
日期：yyyy-MM-dd
时间：HH:mm:ss
带毫秒的时间：HH:mm:ss.SSS
日期和时间：yyyy-MM-dd'T'HH:mm:ss
带毫秒的日期和时间：yyyy-MM-dd'T'HH:mm:ss.SSS
```

借助`java.time.format.DateTimeFormatter`​来规范格式

```java
package org.vaadin.marcus.spring;

/**
 * @author Z41sArrebol
 * @date 2025/2/22
 */
public class LocalDateTimeTest {
    public static void main(String[] args) {  
//调用方法打印当前时间  
        LocalTime t1 = LocalTime.now();
        System.out.println(t1);
        LocalDateTime now = LocalDateTime.now();
        System.out.println(now);
//规范格式（这里dtf是一个样式的名称）
        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm:ss");
        String nowStr = dtf.format(now);
        System.out.println(nowStr);
        String str1 = "2021-05-01 21:30:00";
//parse:将原有的日期按照一个样式进行转化，这里用的日期是str1，格式是dtf
        LocalDateTime time = LocalDateTime.parse(str1, dtf);
        System.out.println(time);
}
```

注意日期必须标准，如2025-02-23而不是2025-2-23，0不能省

相关方法还有：

​`time.toString()`​把time的数据变成字符串

​`plusDays(天数) `​执行天数的相加

如：`LocalDate leaveTime = time.plusDays(days);`​

还有更多计算：

```java
import java.time.LocalDate;
public class DateTest10 {
  public static void main(String[] args) {
    LocalDate now = LocalDate.now();
    System.out.println("当前：" + now.toString());
    System.out.println("加法运算");
    System.out.println("加1天：" + now.plusDays(1));
    System.out.println("加1周：" + now.plusWeeks(1));
    System.out.println("加1月：" + now.plusMonths(1));
    System.out.println("加1年：" + now.plusYears(1));
    System.out.println("减法运算");
    System.out.println("减1天：" + now.minusDays(1));
    System.out.println("减1周：" + now.minusWeeks(1));
    System.out.println("减1月：" + now.minusMonths(1));
    System.out.println("减1年：" + now.minusYears(1));
  }
}
```

```java
import java.time.LocalDate;

public class DateTest11 {

  public static void main(String[] args) {
    LocalDate now = LocalDate.now();

    // 可以对两个 LocalDate 进行比较，
    // 可以判断一个日期是否在另一个日期之前或之后，
    // 或者判断两个日期是否是同年同月同日。

    boolean isBefore = now.minusDays(1).isBefore(LocalDate.now());
    System.out.println("是否在当天之前：" + isBefore);

    boolean isAfter = now.plusDays(1).isAfter(LocalDate.now());
    System.out.println("是否在当天之后：" + isAfter);

    // 判断是否是当天
    boolean sameDate = now.isEqual(LocalDate.now());
    System.out.println("是否在当天：" + sameDate);
  }

}
```

## 集合，接口

接口在JAVA编程语言中是一个抽象类型，是抽象方法的集合，接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。

ArrayList继承了AbstractList，并实现了java.util.List接口

java.util.List有几个最常用的方法：

```java
public void add(int index,E element):将指定的元素，添加到该集合中的指定位置上。
public E get(int index):返回集合中指定位置的元素。
public E remove(int index):移除列表中指定位置的元素，返回的是被移除的元素。
public int size():取得集合里元素的个数，list.size( );
public void clear():删除所有元素,list.clear();
```

在 Java 当中接口是不能直接被实例化的，所以如果想要创建一个接口类型的变量，得需要 new 实现类。`java.util.List`​ 接口的实现类一般会用 `java.util.ArrayList`​

比如：

```java
// 创建一个字符串集合，用于存储多个字符串
List<String> strings = new ArrayList<>();
// 创建一个用户集合，用于存储多个用户信息
List<User> users = new ArrayList<>();
```

示例：

```java
package org.vaadin.marcus.spring;
import java.util.ArrayList;
import java.util.List;
public class ListTest {
    public static void main(String[] args) {
        List<String> userName = new ArrayList<>();
        List<String> password = new ArrayList<>();
        userName.add("admin");
        password.add("admin123");
        userName.add("nb");
        password.add("nb123");
        userName.add("test");
        password.add("test123");
        String userName1 = userName.get(2);
        System.out.println(userName1);
    }
}
```

## 循环

```java
package org.vaadin.marcus.spring;
import org.vaadin.marcus.spring.model.Score;
import java.util.ArrayList;
import java.util.List;

public class ListTest {
    public static void main(String[] args) {
        List<Score> scores = new ArrayList<>();
//<Score>是类名，下面的操作是在进行实例化
        Score score = new Score();
        score.setName("joe");
        score.setScore(80.0);
        scores.add(score);

        score = new Score();
        score.setName("alice");
        score.setScore(87.0);
        scores.add(score);

        score = new Score();
        score.setName("hali");
        score.setScore(70.0);
        scores.add(score);
//这里为什么不需要创建多个变量score123呢，因为每次score的值被更新之后，就被上传了，再被更新
        Double total = 0;
        for (Score score1 : scores) {
            total += score1.getScore();
        }
        System.out.println(total);
        System.out.println(total/scores.size());
    }
}
```

## 常量

1.常量本质上也是一种类变量，只是多了一个 static 关键字常量也可以叫做静态变量。

2.常量可以通过：ClassName.VariableName的方式访问。

3.常量在第一次被访问时创建，在程序结束时销毁。

4.不论一个类创建了多少个对象，类只拥有类变量的一份拷贝。

# 数据存储 文件操作

常量数据是存储在 Java 的内存里，所以当程序重启后内存的数据会丢失的。如果不想数据丢失，就需要考虑数据持久化，一般持久化有两种手段：

1.储存在文件系统里。

2.存储在数据库里。

## 异常

### 异常发生的原因通常有：

1.用户输入了非法数据。

2.要打开的文件不存在。

3.网络通信时连接中断，或者JVM内存溢出。

这些异常有的是因为用户错误引起，有的是程序错误引起的，还有其它一些是因为物理错误引起的。

### 三种异常：

1.检查性异常（在编译时不会被忽略，例如打开一个不存在的文件）

2.运行性异常（在编译的时候会被忽略）

3.错误（脱离程序员控制，例如内存溢出）

### 捕获异常：

使用 try 和 catch 关键字可以捕获异常。try/catch 代码块放在异常可能发生的地方。

try/catch代码块中的代码称为保护代码，语法如下：

‍

```java
try{
    // 程序代码
} catch(ExceptionName e1){
    // Catch 块
}
```

Catch 语句包含要捕获异常类型的声明。

当保护代码块中发生一个异常时，try 后面的 catch 块就会被检查。

如果发生的异常包含在 catch 块中，异常会被传递到该 catch 块。

**意义：出现异常后程序可以继续运行**

## 文件读

引入Apache Commons-io依赖（pom.xml）

```java
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.10.0</version>
</dependency>
```

```java
import org.apache.commons.io.FileUtils;
import java.io.File;
import java.io.IOException;

public class FileTest {
    public static void main(String[] args) {
        File nameFile = new File("./111.txt");
        try {
            String content = FileUtils.readFileToString(nameFile, "utf-8");
            System.out.println(content);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
