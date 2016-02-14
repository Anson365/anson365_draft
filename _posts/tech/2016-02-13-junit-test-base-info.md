---
layout: post
title: Junit单元测试相关知识整理
category: 技术
tags: junit
description: junit基本知识整理
---

### junit4 的使用   
* @ BeforeClass 在整个测试执行之前执行，所属方法必须是 static void 类型；一般用于加载大型文件或者类，以及整个测试用例执行的环境搭建；
* @ Before 在每一个测试用例执行之前执行，所属方法必须是void类型；一般用于小型文件的加载或者对象的实例化；
* @ After 在每个测试用例执行之后执行，所属方法必须是void类型；一般用于关闭或者销毁某些可能会对下次测试造成影响的功能或者参数；
* @ AfterClass 在整个测试完成后执行，所属方法必须是 static void 类型；功能及作用与@BeforeClass相对应；
* @Ignore 在执行单元测试时忽略的测试方法，即JVM见到此方法后不会对其所属方法进行执行；用在测试用例尚未编写完成或者其他不想执行此测试用例的情况下；
* @ Test 即测试用例。@Test(TimeOut=1000)即在一秒内未执行完此测试用例则执行失败error，@ Test(expected=Exception.class)即执行测试用例时未抛出异常则测试error，抛出的异常与Exception的类型不符则执行failure；
* Assert 即将返回结果与预测结果进行判断。常用的assert有assertTrue、assertFalse、assertEqual、assertNotEqual、assertSame、assertNull、assertNotNull等还有专门的assert包提供使用。

注：
```
    测试error与，测试failure之间的区别
    Error：即错误，一般发生在测试用例执行过程中，代表程序本身的错误；
    Failure：即失败，一般发生在测试执行之后，即测试实际返回的结果与测试设想的结果不一致。
    用@Ignore标注的测试方法也同样会返回此结果。
```

### Mock的使用

我们在进行单元测试时，存在某些与测试过程不相干但是却在测试用例的参数构建或者结果返回中扮演着非常重要的角色的类。有些类构建起来十分的繁琐或者我们并不知道它的内部实现细节，这个时候对这个类进行Mock就显得十分的高效和便易了。

一些常用的Mock区别与特点

1.  easyMock：当时因为其结构比较固定和繁琐并没有做太深入的研究；
2.  JMock：相对于easyMock结构比较简单，但是对结果进行预期设定的时候比较麻烦，而且只能对接口进行Mock。如果对实体类进行Mock时需要较为复杂的配置。但是其Make Up（未做深入研究）可以对类中final类型的方法进行Mock。
3.  Mockito ：此Mock工具使用起来非常的简单。直接配置Jar包，引用后方可使用。而且预期和对类中方法的Mock十分的简单。但不足之处是无法对类中的final方法进行Mock。支持java的annotation。
```
Mockito中@Mock与@Spy的区别：

Mock对类进行了模拟，对其使用时可以让类中的方法完全按照测试人员自己的意图进行。
而Spy则是相当于对类进行了复制，调用类中方法的时候不能对原有的方法进行修改。
模拟前端页面的请求
```
由于前端页面的相关类中大部分都是一些容器，构建起来非常的麻烦。使用Mcok的方式也无法对前端页面的参数进行构建，换句话说Mock的类也许就根本没有setter这个方法。这个时候用通用思路处理起来就感到十分的棘手。下面是我研究了的几种方法：

1.  使用catcus：它是基于Junit 3.8开发的，好像junit4也有一定的兼容性。前段时间主要花在了对它的研究。但是后来在具体使用的时候才发现catcus使用起来十分的麻烦。不仅要对Tomcat进行单独的配置，还不支持Eclipse等开发工具。需要将代码单独生成class文件，放在指定的目录下面然后在进行运行。后来把这个工具给放弃了。
2.  使用jetty：jetty相对于前者使用起来就非常的方便了。在Eclipse中配置相应的环境直接就可以用了。而且测试代码跑的时候直接可以用Eclipse自带的junit debug/runner
3.  HttpUnit：直接模拟前端请求。具体的使用没有做太深入的研究，不过使用其来还是比较方便的。

### 其他的辅助功能

为了验证SQL语句的正确性，我们选择了直接对数据库进行交互的方式进行。
为了减轻对数据库的干扰。我们要求测试用例按照一定的顺序执行如：C—R—U—D。所以我们在测试类前面添加了
```
@FixMethodOrder(MethodSorters.NAME_ASCENDING)
```
使Test用例按方法名称字母顺序执行
