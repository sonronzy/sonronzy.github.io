---
title: Java —— 异常体系
author: sonronzy
date: 2023-12-04 06:27:49
update: 2023-12-04 06:27:49
home_cover:
home_cover_height:
post_cover:
post_cover_height:
categories:
- [Java,Exception]
tags:
- Java
- Exception
---

<br>

# 异常概述

## 什么是程序的异常

- **异常** ：指的是程序在执行过程中，出现的非正常情况，如果不处理最终会导致JVM的非正常停止

<br>

<br>

## 异常的抛出机制

- Java中把不同的异常用不同的类表示：
  - 一旦发生某种异常，就创建该异常类型的对象，并且抛出（throw）。然后程序员可以捕获(catch)到这个异常对象，并处理；
  - 如果没有捕获(catch)这个异常对象，那么这个异常对象将会导致程序终止。

<img src="异常产生过程.png" title="异常产生过程" alt="图片无法正常加载展示！" width="80%" height="80%" >

<br>

<br>

## 如何对待异常

- 对于程序出现的异常，一般有两种解决方法：
  - （1）遇到错误就终止程序的运行
  - （2）程序员在编写程序时，就充分考虑到各种可能发生的异常和错误，极力预防和避免。实在无法避免的，要编写相应的代码**进行异常的检测、以及异常的处理，保证代码的健壮性**。

<br>

<br>

<br>

# Java异常体系

##  Throwable

- `java.lang.Throwable` 类是Java程序执行过程中发生的异常事件对应的类的根父类。

- **Throwable**可分为两类：**Error**和**Exception**。分别对应着`java.lang.Error`与`java.lang.Exception`两个类。

- **Throwable中的常用方法：**

  * `public void printStackTrace()`：打印异常的详细信息。包含了异常的类型、异常的原因、异常出现的位置、在开发和调试阶段都得使用printStackTrace。


  * `public String getMessage()`：获取发生异常的原因。



### Error

- Java虚拟机无法解决的严重问题。如：JVM系统内部错误、资源耗尽等严重情况。一般不编写针对性的代码进行处理
  - 例如：StackOverflowError（栈内存溢出）和OutOfMemoryError（堆内存溢出，简称OOM）。



### Exception

- 其它因编程错误或偶然的外在因素导致的一般性问题，需要使用针对性的代码进行处理，使程序继续运行。否则一旦发生异常，程序也会挂掉。例如：
  - 空指针访问
  - 试图读取不存在的文件
  - 网络连接中断
  - 数组角标越界

<br>

<br>

## 编译时异常和运行时异常

<img src="编译时过程和运行时过程.png" title="编译时过程和运行时过程" alt="图片无法正常加载展示！" width="100%" height="100%" >

<br>

- Java程序的执行分为**编译时过程**和**运行时过程**。有的错误只有在运行时才会发生。比如：除数为0，数组下标越界等。根据异常可能出现的阶段，可以将异常分为：
  - **编译时期异常**（即checked异常、受检异常）：在代码编译阶段，编译器就能明确警示当前代码可能发生（不是一定发生）xx异常，并明确督促程序员提前编写处理它的代码。如果程序员没有编写对应的异常处理代码，则编译器就会直接判定编译失败，从而不能生成字节码文件。通常，这类异常的发生不是由程序员的代码引起的，或者不是靠加简单判断就可以避免的，例如：FileNotFoundException（文件找不到异常）。
  - **运行时期异常**（即runtime异常、unchecked异常、非受检异常）：在代码编译阶段，编译器完全不做任何检查，无论该异常是否会发生，编译器都不给出任何提示。只有等代码运行起来并确实发生了xx异常，它才能被发现。通常，这类异常是由程序员的代码编写不当引起的，只要稍加判断，或者细心检查就可以避免。
    - **java.lang.RuntimeException**类及它的子类都是运行时异常。比如：ArrayIndexOutOfBoundsException数组下标越界异常，ClassCastException类型转换异常。

<br>

<img src="异常体系结构.png" title="异常体系结构" alt="图片无法正常加载展示！" width="60%" height="60%" >

<br>

<br>

<br>

# 常见的错误和异常

## Error

- 最常见的就是VirtualMachineError，它有两个经典的子类：StackOverflowError、OutOfMemoryError。

```java
public class TestStackOverflowError {
    @Test
    public void test01(){
        //StackOverflowError
        recursion();
    }

    public void recursion(){ //递归方法
        recursion(); 
    }
}
```

```java
public class TestOutOfMemoryError {
    @Test
    public void test02(){
        //OutOfMemoryError
        //方式一：
        int[] arr = new int[Integer.MAX_VALUE];
    }
    @Test
    public void test03(){
        //OutOfMemoryError
        //方式二：
        StringBuilder s = new StringBuilder();
        while(true){
            s.append("atguigu");
        }
    }
}
```

<br>

<br>

## 运行时异常

```java
public class TestRuntimeException {
    @Test
    public void test01(){
        //NullPointerException
        int[][] arr = new int[3][];
        System.out.println(arr[0].length);
    }

    @Test
    public void test02(){
        //ClassCastException
        Object obj = 15;
        String str = (String) obj;
    }

    @Test
    public void test03(){
        //ArrayIndexOutOfBoundsException
        int[] arr = new int[5];
        for (int i = 1; i <= 5; i++) {
            System.out.println(arr[i]);
        }
    }

    @Test
    public void test04(){
        //InputMismatchException
        Scanner input = new Scanner(System.in);
        System.out.print("请输入一个整数：");//输入非整数
        int num = input.nextInt();
        input.close();
    }

    @Test
    public void test05(){
        int a = 1;
        int b = 0;
        //ArithmeticException
        System.out.println(a/b);
    }
}

```

<br>

<br>

## 编译时异常

```java
public class TestCheckedException {
    @Test
    public void test06() {
        Thread.sleep(1000);//休眠1秒  InterruptedException
    }

    @Test
    public void test07(){
        Class c = Class.forName("java.lang.String");//ClassNotFoundException
    }

    @Test
    public void test08() {
        Connection conn = DriverManager.getConnection("....");  //SQLException
    }
    @Test
    public void test09()  {
        FileInputStream fis = new FileInputStream("Java秘籍.txt"); //FileNotFoundException
    }
    @Test
    public void test10() {
        File file = new File("Java秘籍.txt");
		FileInputStream fis = new FileInputStream(file);//FileNotFoundException
		int b = fis.read();//IOException
		while(b != -1){
			System.out.print((char)b);
			b = fis.read();//IOException
		}
		
		fis.close();//IOException
    }
}
```

<br>

<br>

<br>

# 异常的处理

## Java的异常处理的抓抛模型

- 程序在执行的过程当中，一旦出现异常，就会在出现异常的代码处生成对应异常类的对象，并将此对象抛出。这个过程称为**抛出(throw)异常**。一旦抛出，此程序就不执行其后的代码了。
- 如果一个方法内抛出异常，该异常对象会被抛给调用者方法中处理。如果异常没有在调用者方法中处理，它继续被抛给这个调用方法的上层方法。这个过程将一直继续下去，直到异常被处理。这一过程称为**捕获(catch)异常**。一旦将异常进行了处理，代码就可以继续执行。
- 如果一个异常回到main()方法，并且main()方法也不处理，则程序运行终止。

<br>

<br>

## 方式1：捕获异常（try-catch-finally）

### try-catch-finally 概述

- **捕获异常语法基本格式如下：**

```java
try{
	......	//可能产生异常的代码
}
catch( 异常类型1 e ){
	......	//当产生异常类型1型异常时的处置措施
}
catch( 异常类型2 e ){
	...... 	//当产生异常类型2型异常时的处置措施
}  
finally{
	...... //无论是否发生异常，都无条件执行的语句
} 
```



- **整体执行过程：**

当某段代码可能发生异常，不管这个异常是编译时异常（受检异常）还是运行时异常（非受检异常），我们都可以使用try块将它括起来，并在try块下面编写catch分支尝试捕获对应的异常对象。

- 如果在程序运行时，try块中的代码没有发生异常，那么catch所有的分支都不执行。
- 如果在程序运行时，try块中的代码发生了异常，根据异常对象的类型，将从上到下选择第一个匹配的catch分支执行。此时try中发生异常的语句下面的代码将不执行，而整个try...catch之后的代码可以继续运行。
- 如果在程序运行时，try块中的代码发生了异常，但是所有catch分支都无法匹配（捕获）这个异常，那么JVM将会终止当前方法的执行，并把异常对象“抛”给调用者。如果调用者不处理，程序就挂了。



- **try**
  - 捕获异常的第一步是用`try{…}`语句块选定捕获异常的范围，将可能出现异常的业务逻辑代码放在try语句块中。




- **catch (Exceptiontype e)**

  - catch分支，分为两个部分，catch()中编写异常类型和异常参数名，{}中编写如果发生了这个异常，要做什么处理的代码。


  - 如果明确知道产生的是何种异常，可以用该异常类作为catch的参数；也可以用其父类作为catch的参数。

    比如：可以用ArithmeticException类作为参数的地方，就可以用RuntimeException类作为参数，或者用所有异常的父类Exception类作为参数。但不能是与ArithmeticException类无关的异常，如NullPointerException（catch中的语句将不会执行）。


  - 每个try语句块可以伴随一个或多个catch语句，用于处理可能产生的不同类型的异常对象。


  - 如果有多个catch分支，并且多个异常类型有父子类关系，必须保证小的子异常类型在上，大的父异常类型在下。否则，报错。


  - catch中常用异常处理的方式

    - `public String getMessage()`：获取异常的描述信息，返回字符串

    - `public void printStackTrace()`：打印异常的跟踪栈信息并输出到控制台。包含了异常的类型、异常的原因、还包括异常出现的位置，在开发和调试阶段，都得使用printStackTrace()。




<img src="异常堆栈信息.png" title="异常堆栈信息" alt="图片无法正常加载展示！" width="80%" height="100%" >



### 使用举例

```java
@Test
public void test1(){
	try{
		String str1 = "atguigu.com";
		str1 = null;
		System.out.println(str1.charAt(0));
	}catch(NullPointerException e){
		//异常的处理方式1
		System.out.println("不好意思，亲~出现了小问题，正在加紧解决...");	
	}catch(ClassCastException e){
		//异常的处理方式2
		System.out.println("出现了类型转换的异常");
	}catch(RuntimeException e){
		//异常的处理方式3
		System.out.println("出现了运行时异常");
	}
	//此处的代码，在异常被处理了以后，是可以正常执行的
	System.out.println("hello");
}
```



### finally使用及举例

<img src="finally执行流程.png" title="图片名称" alt="图片无法正常加载展示！" width="70%" height="70%" >







<br>

<br>

## 方式2：声明抛出异常类型（throws）





<br>

<br>

<br>

# Reference

- []()





<br>

<br>

<br>

# Remark

> 1. xxx需要再深入学习

```html
<font color=red></font>
![]()
<img src="finally执行流程.png" title="图片名称" alt="图片无法正常加载展示！" width="100%" height="100%" >
<img src="" width="90%">
<img src="" width="70%">
****
```

<br>

<br>
