---
layout: post
title: 包访问权限问题探究
categories: [Tech]
---

{{ page.title }}
================

<p class="meta">20 October 2016 - ShangHai</p>

今天在写深度克隆方法时遇到了一个protected访问权限的问题。

先看几个例子：

{% highlight java %}
package com.A;
class Professor
{
    String name;
    int age;
    Professor(String name,int age)
    {
        this.name=name;
        this.age=age;
    }
}

public class Test {

    public static void main(String args[]){
        Professor pro = new Professor("name", 23);
        pro.clone(); //java: clone()可以在java.lang.Object中访问protected
    }
}

{% endhighlight %}

Object.clone()是protected方法。这说明，该方法可以被同包（java.lang）下和它（java.lang.Object）的子类访问。

现在让Test继承自Professor，再看个例子：

{% highlight java %}
package com.A;
class Professor
{
    String name;
    int age;
    public Professor(){}
    Professor(String name,int age)
    {
        this.name=name;
        this.age=age;
    }
}

public class Test extends Professor{

    public static void main(String args[]){
        Professor pro = new Professor("name", 23);
        pro.clone();  //java: clone()可以在java.lang.Object中访问protected
    }
}

{% endhighlight %}

在子类中通过父类对象访问父类的clone()方法发现仍然出现同样的错误。

现在在Professor中覆盖Object类的clone()方法，例子：

{% highlight java %}
package com.A;
class Professor
{
    String name;
    int age;
    public Professor(){}
    Professor(String name,int age)
    {
        this.name=name;
        this.age=age;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class Test {

    public static void main(String args[]) throws Exception{
        Professor pro = new Professor("name", 23);
        pro.clone();   //compiler OK
    }
}

{% endhighlight %}

再来看一个例子，两个类在不同的包中：

{% highlight java %}
package com.A;
class Professor
{
    String name;
    int age;
    public Professor(){}
    Professor(String name,int age)
    {
        this.name=name;
        this.age=age;
    }

    protected Object clone() throws CloneNotSupportedException {
	   return super.clone();
	}
}

package com.B;
public class Test extends Professor{

    public static void main(String args[]){
        Professor pro = new Professor("name", 23);
        pro.clone();  //java: clone()可以在java.lang.Object中访问protected

        Test test = new Test();
		test.clone();  //compiler OK
    }
}

{% endhighlight %}

意想不到的结果，protected方法不是可以被继承类访问吗？

必须明确，类Test确实是继承了类Professor（包括它的clone方法），所以在类Test中可以调用自己的clone方法。但类Professor的protected方法对其不同包子类Test来说，是不可见的。

总结下：

protected 访问权限修饰，可在这两种方式下可访问：

1、在当前子类中访问父类的protected方法。

2、不在当前子类中访问该方法，但该方法所在类与调用该方法所在类在同一个包下

国内的很多Java书籍在介绍访问权限时，一般都这样描述，太过草率：

| 		        		| public        | protected	    | default       | private  |
| --------------------  |:-------------:|:-------------:|:-------------:|---------:|
| 同类	        		|	T			|T				|			   T| 		  T|
| 同包	        		|	T			|T				|			   T| 		   |
| 子类(不同包)   			|	T			|T				|			   | 		  |
| 不同包中无继承关系的类   |	T			|				|			   | 		  |