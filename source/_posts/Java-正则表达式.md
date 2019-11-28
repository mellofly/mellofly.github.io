---
title: Java_正则表达式
author: mellofly
tags:
  - Java
  - String
categories:
  - Java
abbrlink: 12588
date: 2019-11-25 19:58:44
---
 
正则表达式：符合一定规则的表达式。
- 作用：用于专门操作字符串。
- 特点：用一些特定的符号来表示一些代码操作，这样就简化书写。
>所以学习正则表达式，就是在学习一些特殊符号的使用。

- 好处：可以简化对字符串的复杂操作。
- 弊端：符号定义越多，正则越长，阅读性越差。

# 具体操作功能：

## 1.匹配：String
matches方法。用规则匹配整个字符串，只要有一处不符合规则，就匹配结束，返回false
正则表达式中的“\”需要进行转义,所以是成对出现的。
字符出现次数：
- ？(一次或一次也没有)
- *（零次或多次）
- +（一次或多次）
- {n}(恰好n次)
- {n,}(至少n次)
- {n,m}(至少n次，但是不超过m次)

```java
//对QQ号码进行校验
//要求：5~15位，0不能开头，只能是数字
//这种方式，使用了String类中的方法，进行组合完成了需求，但是代码过于复杂。
public static void checkQQ(){
    String qq="";
    int len = qq.length();
    if(len>5&&len<=15){
        if(!qq.startWith("0")){
            /*
            char[] arr = qq.toCharArray();
            boolean flag= true;
            for(int x=0;x<arr.length;x++){
                if(!(arr[x]>'0'&&arr[x]<='9')){
                     flag=false;
                     break;
                }
            }
            if(flag){
                System.out.println("qq:"+qq);
            }else{
                System.out.println("出现非法字符");
            }
            */
            //替换为：
            try{
                long l = Long.parseLong(qq);
                System.out.println("qq:"+qq);
            }
            catch(NumberFormatException e){
                SYstem.out.println("出现非法字符");
            }
        }
        else{
            System.out.println("0不能开头");
        }
    }
    else{
        System.out.println("长度不对");
    }
}
//使用下面的方法比较简单：regex(正则表达式)
public static void checkQQ_simple(){
    String qq ="255616548";
    String regex ="[1-9][0-9]{4,14}";//正则表达式
    boolean flag = qq.matches(regex);
    if(flag){
        System.out.println(qq+" ...is ok");
    }
    else{
        System.out.println(qq+"...不合法");
    }
}

```

## 2.切割：
```java
public static void splitDemo(){
    //String str ="zhangsan  lisi   wangwu";
    //String reg=" +";//按照多个空格来进行切割
    //String str ="zhangsan.lisi.wangwu";
    //String reg ="\\.";//用点来进行切割
    //String str ="c:\\abc\\a.txt";
    //String reg ="\\\\";//用\\来进行切割
    String str ="ekjfokksdjfoiewdd";
    String reg ="(.)\\1";//用叠词来进行切割()叫做组 叠词数量不定"(.)\\1+"
    \\为了可以让规则的结果被重用，可以将规则封装成一个组，用()来完成，
    \\组的出现都有编号。
    \\从1开始。想要使用已有的组可以通过\n(n就是组的编号)的形式来获取。
    String[]arr = str.split(reg);
    System.out.println(arr.length);
    for(String s : arr){
        System.out.println(s);
    }
}
```
## 3.替换
```java
public static void replace(){
    //String str ="dfw902839082193jfefioe21093";
    //String reg ="\\d{5,}";//数字替换
    String str ="skdlfaaafewiodddfeoi";
    //String reg ="(.)\\1+";//叠词替换为&
    //String newStr="&";
    String reg="(.)\\1+";//叠词替换为叠词中的单个词
    String newStr ="$1";//$1获取前一个规则中的第一个组。
    String arr = str.replaceAll(reg,newStr);
    System.out.println(arr);
}
```

## 4.获取：
>按照规则将指定规则的子串在字符串中获取出来。

操作步骤：
- 1.将正则表达式封装成对象。
- 2.让正则对象和要操作的字符串相关联。
- 3.关联后，获取正则匹配引擎。
- 4.通过引擎对符合规则的子串进行操作，比如取出。
```java
public static void getDemo(){
    String str = "abc djfewo dsjfo dfd fjei dfs";
    String reg="\\b[a-z]{3}\\b";//查找只有三个单词的，需要加一个边界\\b
    //将规则封装成对象。
    Pattern p = Pattern.compile(reg);
    //让正则对象和要作用的字符串进行关联。获取匹配器对象。
    Matcher m = p.matcher(str);
    //System.out.println(m.matches());//其实String类中的matches方法。用的就是Pattern
                                    //和Matcher对象来完成的。只不过被String的方法封
                                   //装后，用起来比较简单，但是功能却单一。
    //group()方法是获取匹配后结果
    //m.find();//将规则作用到字符串上，并进行符合规则的子串查找。是group()方法的前提。
    //System.out.println(m.group());匹配之前先要使用find()进行查找。
    while(m.find()){
        System.out.println(m.group);
        System.out.println(m.start()+"....."+m.end());//取得是字串的开头和结尾。
    }
                                       
}
```
练习：
```java
//邮箱匹配
public static void checkMail(){
    String mail = "abc12@sunline.orc.com";
    String reg="[a-zA-Z0-9_]+@[a-zA-Z0-9]+(\\.[a-zA-Z]+){1,3}";//较为精确的匹配。
    reg="\\w+@\\w+(\\.\\w+)+";//相对不太精确的匹配
    System.out.println(mail.matches(reg));
}
```

