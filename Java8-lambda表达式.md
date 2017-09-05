## java 8 Lambda表达式


### 介绍
 
本篇文章主要是RxJava官方文档的辅料，里面太多的Lambda表达式，没有一些了解的确难以看懂，因为是主要的重心还是落在RxJava上，所以本文概述一下如何在Android Studio中使用以及各个表达式的意思，还有与Rxjava及其相似的stream。[官方文档](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/Lambda-QuickStart/index.html#)如何描述Lambda表达式：

>  Lambda expressions are a new and important feature included in Java SE 8. They provide a clear and concise way to represent one method interface using an expression. Lambda expressions also improve the Collection libraries making it easier to iterate through, filter, and extract data from a Collection. In addition, new concurrency features improve performance in multicore environments.

这段话指出了在java8中lambda表达式三个主要的作用：简洁的方式呈现接口；提高集合迭代，过滤和扩展的性能；提高多核环境下并发的性能。具体的翻译还是大家自行去理解，中英转换肯定会出现一定的偏差。

### 软硬件环境

* JDK 8
* NetBeans 7.4 （这个只是IDE 在Android Studio中同样是可以使用的）

### Lambda应用在那些场景呢？因为不知道怎么翻译合适就直接上英语了.

#### Anonymous Inner Class
	
 匿名内部类，用过java的人这个应该都知道就不再赘述

	// 匿名内部类
	JButton jb1 = new JButton();
	jb1.addActionListener(new ActionListener() {
	    @Override
	    public void actionPerformed(ActionEvent e) {
	        System.out.println("xxx");
	    }
	});
   
    // lambda表达式的方式
	JButton jb = new JButton();
	jb.addActionListener((e) -> System.out.println());

#### Functional Interface

在java中，一个interface里面只有一个需要实现的方法，这种形式的接口就被称作为Functional Interface,也被叫做SAM.(Single Abstract Method)
	public interface ActionListener extends EventListener {
	
	    /**
	     * Invoked when an action occurs.
	     */
	    public void actionPerformed(ActionEvent e);
	
	}
 

#### Lambda Expression Syntax 

 λ表达式的语法，通常λ表达式可以将5行代码，转换成一行代码来表示，上面的匿名内部类便是例子，λ表达式的语法由如下三个部分组成：

|参数列表|箭头|方法体|
| ------------- |:-------------|:-------------|
|（int x,int y）|->|x + y|
