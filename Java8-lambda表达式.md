## java 8 Lambda表达式


### 介绍
 
本篇文章主要是RxJava官方文档的辅料，里面太多的Lambda表达式，没有一些了解的确难以看懂，因为是主要的重心还是落在RxJava上，所以本文概述一下如何在Android Studio中使用以及各个表达式的意思，还有与Rxjava及其相似的stream。[官方文档](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/Lambda-QuickStart/index.html#)如何描述Lambda表达式：

>  Lambda expressions are a new and important feature included in Java SE 8. They provide a clear and concise way to represent one method interface using an expression. Lambda expressions also improve the Collection libraries making it easier to iterate through, filter, and extract data from a Collection. In addition, new concurrency features improve performance in multicore environments.

这段话指出了在java8中lambda表达式三个主要的作用：简洁的方式呈现接口；提高集合迭代，过滤和扩展的性能；提高多核环境下并发的性能。具体的翻译还是大家自行去理解，中英转换肯定会出现一定的偏差。

### 软硬件环境

* JDK 8
* NetBeans 7.4 （这个只是IDE 在Android Studio中同样是可以使用的）

## Lambda应用在那些场景呢？因为不知道怎么翻译合适就直接上英语了.

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


## lambda表达式实例


#### Listener Lambda
	private static void listenerLambda() {
        //        Listener Lambda
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
    }


#### Comparator Lambda

    private static void compatorLambda() {
        // sort with inner class
        List<Person> personList = new ArrayList<>();
        personList.add(new Person("zk"));
        personList.add(new Person("ak"));


        System.out.println("sort with inner class");
        Collections.sort(personList, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                // 按照字典序排列 person
                return o1.getSurName().compareTo(o2.getSurName());
            }
        });

        for (Person p:
                personList) {
            System.out.println(p.getSurName());
        }


        System.out.println("sort with lambda");
        // sort with lambda
        Collections.sort(personList, (p1,p2)->p1.getSurName().compareTo(p2.getSurName()));
        for (Person p:
                personList) {
            System.out.println(p.getSurName());
        }
    }


#### Runnable Lambda

    private static void runnableLambda() {
        // 传统模式
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("Tran  A");
                System.out.println("Tran  B");
            }
        };

        // lambda
        Runnable r2 = () -> System.out.print("Lambda  A");
        System.out.print("Lambda  B");

        r1.run();
        r2.run();
    }

## Improving Code with Lambda Expression 

 使用Lambda优化代码。代码优化这件事可轻可重，像我写代码的主要目的就是奔着功能去，一往无前，有bug回头改,往往都是一去不返。代码优化有很多方面，像6大设计原则，23种设计模式等等，今天得例子要验证得Principle叫做 --- 'Don't Repeat Yourself ' 简称 (DRY)，不得不说所有得原则全称都不如简称有格调。

 那下面我们就来验证一下lambda相对于原有得做法到底 DRY 在哪里：

> 1.场景 
  
 有一群人来面试,只招聘司机、应征入伍、飞行员，面试得条件如下：
 	
	司机 ： 年纪超过16
    入伍 ： 男性年龄在18-25之间
    飞行员：年龄在23 到 65之间

 Person 字段 

	public class Person {
    private String givenName;
    private String surName;
    private int age;
    private Gender gender;
    private String eMail;
    private String phone;
    private String address;	

> 2.创建person List

   怎么创建比较随意，我这里就按照官网得方式来创建（不熟悉Builder模式的可以看我的Rxjava学习计划中的附录里面有设计模式之禅的下载链接）

   	public static List<Person> createShortList() {
        List<Person> people = new ArrayList<>();
        people.add(
                new Person.Builder().
                        givenName("Bob")
                        .surName("Baker")
                        .age(21)
                        .gender(Gender.MALE)
                        .email("bob.baker@example.com")
                        .phoneNumber("201-121-4678")
                        .address("44 4th St, Smallville, KS 12333")
                        .build()
        );
        people.add(
                new Person.Builder()
                        .givenName("Jane")
                        .surName("Doe")
                        .age(25)
                        .gender(Gender.FEMALE)
                        .email("jane.doe@example.com")
                        .phoneNumber("202-123-4678")
                        .address("33 3rd St, Smallville, KS 12333")
                        .build()
        );
        people.add(
                new Person.Builder()
                        .givenName("John")
                        .surName("Doe")
                        .age(25)
                        .gender(Gender.MALE)
                        .email("john.doe@example.com")
                        .phoneNumber("202-123-4678")
                        .address("33 3rd St, Smallville, KS 12333")
                        .build()
        );

        return people;
    }

> 3.选出你所要挑选的满足条件的人

	public class RoboContactMethods {

	    public void callDrivers(List<Person> pl) {
	        for (Person p : pl) {
	            if (p.getAge() >= 16) {
	                roboCall(p);
	            }
	        }
	    }
	
	    public void emailDraftees(List<Person> pl) {
	        for (Person p : pl) {
	            if (p.getAge() >= 18 && p.getAge() <= 25 && p.getGender() == Gender.MALE) {
	                roboEmail(p);
	            }
	        }
	    }
	
	    public void mailPilots(List<Person> pl) {
	        for (Person p : pl) {
	            if (p.getAge() >= 23 && p.getAge() <= 65) {
	                roboMail(p);
	            }
	        }
	    }
	
	    public void roboCall(Person p) {
	        System.out.println("Calling " + p.getGivenName() + " " + p.getSurName() + " age " + p.getAge() + " at " + p.getPhone());
	    }
	
	    public void roboEmail(Person p) {
	        System.out.println("EMailing " + p.getGivenName() + " " + p.getSurName() + " age " + p.getAge() + " at " + p.geteMail());
	    }
	
	    public void roboMail(Person p) {
	        System.out.println("Mailing " + p.getGivenName() + " " + p.getSurName() + " age " + p.getAge() + " at " + p.getAddress());
	    }

	}

我看这段代码是没问题的，for循环迭代选出满足条件的人，完全没毛病；那我们来看看官方的犊子是怎么演的。

* 没有遵循DRY原则
	- 每个方法的选择条件必须重写
	- 每个方法都重复循环
* 需要大量方法来实现每个用例
* 上述代码不灵活，如果招聘条件发生变化，那么将引起大量的更改，可维护性不强


>4.第一次重构
	
 这次我们将上述的几个问题列入考虑，其实，我感觉我的翻译貌似出了点小差错，或者是我没明白他是什么意思，除了最后一项可以理解之外，其他的情况没弄懂是什么意思，再或者是本身语言的限制只能优化最后一项，那么下面看看，该如何重构。

	代码块 ： 先空着

 搜索条件被封装进方法中，较之前的代码而言确实有一定的提升，条件可以被反复利用并且可以更方便的更改。但是，还是有很多重复的地方，是不是没有更好的办法呢？（······肯定是有的，要不我说个球）

 在使用lambda之前先看看还有什么能改进的或者是为lambda做一些铺垫。上述的代码，其实并不符合开闭原则（OCP -- 原则上对修改关闭，对扩展开放），这段程序可能发生改变是招聘条件，那么我们让调用程序自己去维护，就更进一步的优化了。
 
 将条件抽象成接口，让调用者自己去维护，接口如下：
 
	 public interface Predicate<T> {
	   public boolean test(T t);
	 }

 RoboContactsLambda.java


  	package com.example.lambda;
  	import java.util.List;
  	import java.util.function.Predicate;

    /**
     * @author MikeW
     */
    public class RoboContactLambda {
        public void phoneContacts(List<Person> pl, Predicate<Person> pred) {
            for (Person p : pl) {
                if (pred.test(p)) {
                    roboCall(p);
                }
            }
        }

        public void emailContacts(List<Person> pl, Predicate<Person> pred) {
            for (Person p : pl) {
                if (pred.test(p)) {
                    roboEmail(p);
                }
            }
        }

        public void mailContacts(List<Person> pl, Predicate<Person> pred) {
            for (Person p : pl) {
                if (pred.test(p)) {
                    roboMail(p);
                }
            }
        }

        public void roboCall(Person p) {
            System.out.println("Calling " + p.getGivenName() + " " + p.getSurName() + " age " + p.getAge() + " at " + p.getPhone());
        }

        public void roboEmail(Person p) {
            System.out.println("EMailing " + p.getGivenName() + " " + p.getSurName() + " age " + p.getAge() + " at " + p.getEmail());
        }

        public void roboMail(Person p) {
            System.out.println("Mailing " + p.getGivenName() + " " + p.getSurName() + " age " + p.getAge() + " at " + p.getAddress());
        }
    }
 
  

// todo 相关的解释

## The java.util.function 包


 function包提供了几种方便去使用lambda的interface，当然不仅仅是给lambda表达式使用，所有诸如上述的优化都是可以考虑用这些方法的，确实我看到这里就有点抑制不住内心的小狂喜，lambda的这些function不就是Rxjava中常用的操作符嘛，这么麻烦的工作算是没白做，就下面的需求，我们来体验一下她的魅力。

* Predicate: A property of the object passed as argument
* Consumer: An action to be performed with the object passed as argument
* Function: Transform a T to a U
* Supplier: Provide an instance of a T (such as a factory)
* UnaryOperator: A unary operator from T -> T
* BinaryOperator: A binary operator from (T, T) -> T


> 输出东方人和西方人的信息
 
  上面的personList（求职者），要将他们的信息分别发给，老板和老板娘，他们一个是中国人，一个是美国人；由于习惯不同，西方习惯姓在后名在前（分别对应Given name 和 surname），中国的习惯我就不提了。

#### 传统的方式

#### 使用Function Interface的方式

#### 输出


## lambda表达式和集合

 体验过Function接口的优点还没完，java 8给我们的惊喜还不止这些。（不这些都不够，我必须是你近旁的一只木棉，根紧握地里，也相触云间 ... 神特么的乱入），lambda基础部分结束后，来看看在集合方面它又有哪些优势。

> 1.添加新的类，将查询条件全部封装进HashMap中
> 2.Looping
> 3.链式调用和过滤器
> 4.getting Lazy

#### stream
 
 


## 结语



 






	






