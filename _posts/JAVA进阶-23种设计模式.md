---
title: JAVA进阶-23种设计模式
date: 2019-02-18 12:22:20
author: fuf
notebook: blog
evernote-version: 0
source: 原创
thumbnail: 
tags:
    - JAVA,设计模式
blogexcerpt:
---

# 23肿设计模式
<!-- more -->
## 一,设计模式的分类
1. 创建型模式：创建对象的时候，隐藏创建逻辑，共五种，工厂方法模式，抽象工厂模式，单例模式，建造模式，原型模式。
2. 结构型模式：七种，适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式。
3. 行为型模式，共十一种：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。
4. 其实还有两类：并发型模式和线程池模式

## 设计模式的六大原则
0. 开闭原则：对扩展开放，对修改关闭
1. 单一职责原则：每个类应该实现单一的职责。
2. 里氏替换原则：任何基类可以出现的地方，子类一定可以出现，子类对父类的方法尽量不要重写和重载
3. 依赖倒换原则：面向编程，依赖于抽象而不是依赖于具体。
4. 接口隔离原则：每个接口不存在子类用不到却必须实现的方法。
5. 迪米特法则：一个类对自己依赖的类知道的越少越好。
6. 合成复用原则：原则是尽量首先使用合成/聚合的方式，而不是使用继承。

## Java的23种设计模式
1. 创建模式：
  
  1. 简单工厂模式
   - 普遍：建立一个工厂类，对实现了同一个接口的一些类进行实例的创建。

  ```  
    //二者的共同接口
     public interface Sender{
         public void Send();
     }

    //创建实现类
     public class MailSender implements Sender{
         @Override
         public void Send(){
                System.out.println("this is mailsender!");  
         }
     }
    public class SmsSender implements Sender {  
    @Override  
    public void Send() {  
        System.out.println("this is sms sender!");  }
    }
    //工厂类
    public class SendFactory{
        public Sender produce(String type) {  
        if ("mail".equals(type)) {  
            return new MailSender();  
        } else if ("sms".equals(type)) {  
            return new SmsSender();  
        } else {  
            System.out.println("请输入正确的类型!");  
            return null;  
        }           }
        }
    }
    //测试类
    public class FactoryTest {  
    public static void main(String[] args) {  
        SendFactory factory = new SendFactory();  
        Sender sender = factory.produce("sms");  
        sender.Send();  
        }
    }
     
     
     
     输出 this is is sms sender！
```




多个静态方法

```
public class SendFactory {  
    public static Sender produceMail(){  
        return new MailSender();  
    }
    public static Sender produceSms(){  
        return new SmsSender();  
    }
}
public class FactoryTest {  
    public static void main(String[] args) {      
        Sender sender = SendFactory.produceMail();  
        sender.Send();  }
}

   
```

**工厂模式适合：凡是出现了大量的产品需要创建，并且具有共同的接口时，可以通过工厂方法模式进行创建。在以上的三种模式中，第一种如果传入的字符串有误，不能正确创建对象，第三种相对于第二种，不需要实例化工厂类，所以，大多数情况下，我们会选用第三种——静态工厂方法模式。**

1. 工厂方法模式
**创建一个工厂接口和创建多个工厂实现类**


2.  抽象工厂模式
工厂方法模式：
- 一个抽象产品类，可以派生出多个具体产品类。   
- 一个抽象工厂类，可以派生出多个具体工厂类。   
- 每个具体工厂类只能创建一个具体产品类的实例。

抽象工厂模式：
- 多个抽象产品类，每个抽象产品类可以派生出多个具体产品类。   
- 一个抽象工厂类，可以派生出多个具体工厂类。   
- 每个具体工厂类可以创建多个具体产品类的实例，也就是创建的是一个产品线下的多个产品。

区别：
- 工厂方法模式只有一个抽象产品类，而抽象工厂模式有多个。   
- 工厂方法模式的具体工厂类只能创建一个具体产品类的实例，而抽象工厂模式可以创建多个。
- 工厂方法创建 "一种" 产品，他的着重点在于"怎么创建"，也就是说如果你开发，你的大量代码很可能围绕着这种产品的构造，初始化这些细节上面。也因为如此，类似的产品之间有很多可以复用的特征，所以会和模版方法相随。 

- 抽象工厂需要创建一些列产品，着重点在于"创建哪些"产品上，也就是说，如果你开发，你的主要任务是划分不同差异的产品线，并且尽量保持每条产品线接口一致，从而可以从同一个抽象工厂继承。

3. 单例模式
Java应用中，单例对象能保证在一个JVM中，该对象只有一个实例存在。这样的模式有几个好处：

- 某些类创建比较频繁，对于一些大型的对象，这是一笔很大的系统开销。

- 省去了new操作符，降低了系统内存的使用频率，减轻GC压力。

- 有些类如交易所的核心交易引擎，控制着交易流程，如果该类可以创建多个的话，系统完全乱了。（比如一个军队出现了多个司令员同时指挥，肯定会乱成一团），所以只有使用单例模式，才能保证核心交易服务器独立控制整个流程。


```
public enum Singleton {  
    INSTANCE;  
    public void whateverMethod() {  
    }  
}
```





4. 定义：
    建造者模式：将一个复杂的对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。
    实用范围
    1、当创建复杂对象的算法应该独立于该对象的组成部分以及它们的装配方式时。
    2、当构造过程必须允许被构造的对象有不同表示时。

    角色
    在这样的设计模式中，有以下几个角色：
    1、Builder：为创建一个产品对象的各个部件指定抽象接口。
    2、ConcreteBuilder：实现Builder的接口以构造和装配该产品的各个部件，定义并明确它所创建的表示，并提供一个检索产品的接口。
    3、Director：构造一个使用Builder接口的对象，指导构建过程。
    4、Product：表示被构造的复杂对象。ConcreteBuilder创建该产品的内部表示并定义它的装配过程，包含定义组成部件的类，包括将这些部件装配成最终产品的接口。
```
    角色Builder：
    public interface PersonBuilder {
        void buildHead();
        void buildBody();
        void buildFoot();
        Person buildPerson();
    }

    角色ConcreteBuilder：
    public class ManBuilder implements PersonBuilder {
        Person person;
        public ManBuilder() {
            person = new Man();
        }
        public void buildbody() {
            person.setBody("建造男人的身体");
        }
        public void buildFoot() {
            person.setFoot("建造男人的脚");
        }
        public void buildHead() {
            person.setHead("建造男人的头");
        }
        public Person buildPerson() {
            return person;
        }
    }

    角色ConcreteBuilder：
    public class WomanBuilder implements PersonBuilder {
        Person person;
        public WomanBuilder() {
            person = new Woman();
        }
        public void buildbody() {
            person.setBody(“建造女人的身体");
        }
        public void buildFoot() {
            person.setFoot(“建造女人的脚");
        }
        public void buildHead() {
            person.setHead(“建造女人的头");
        }
        public Person buildPerson() {
            return person;
        }
    }

    角色Director：
    public class PersonDirector {
        public Person constructPerson(PersonBuilder pb) {
            pb.buildHead();
            pb.buildBody();
            pb.buildFoot();
            return pb.buildPerson();
        }
    }

    角色Product：
    public class Person {
        private String head;
        private String body;
        private String foot;
    
        public String getHead() {
            return head;
        }
        public void setHead(String head) {
            this.head = head;
        }
        public String getBody() {
            return body;
        }
        public void setBody(String body) {
            this.body = body;
        }
        public String getFoot() {
            return foot;
        }
        public void setFoot(String foot) {
            this.foot = foot;
        }
    }
    public class Man extends Person {
        public Man(){
            System.out.println(“开始建造男人");
        }
    }
    public class Woman extends Person {
        public Woman(){
            System.out.println(“开始建造女人");
        }
    }

    测试：
    public class Test{
        public static void main(String[] args) {
            PersonDirector pd = new PersonDirector();
            Person womanPerson = pd.constructPerson(new ManBuilder());
            Person manPerson = pd.constructPerson(new WomanBuilder());
        }
    }
```
    建造者模式在使用过程中可以演化出多种形式：
    如果具体的被建造对象只有一个的话，可以省略抽象的Builder和Director，让ConcreteBuilder自己扮演指导者和建造者双重角色，甚至ConcreteBuilder也可以放到Product里面实现。
    在《Effective Java》书中第二条，就提到“遇到多个构造器参数时要考虑用构建器”，其实这里的构建器就属于建造者模式，只是里面把四个角色都放到具体产品里面了。

    上面例子如果只制造男人，演化后如下：
    public class Man {
        private String head;
        private String body;
        private String foot;
    
        public String getHead() {
            return head;
        }
        public void setHead(String head) {
            this.head = head;
        }
        public String getBody() {
            return body;
        }
        public void setBody(String body) {
            this.body = body;
        }
        public String getFoot() {
            return foot;
        }
        public void setFoot(String foot) {
            this.foot = foot;
        }
    }

    public class ManBuilder{
        Man man;
        public ManBuilder() {
            man = new Man();
        }
        public void buildbody() {
            man.setBody("建造男人的身体");
        }
        public void buildFoot() {
            man.setFoot("建造男人的脚");
        }
        public void buildHead() {
            man.setHead("建造男人的头");
        }
        public Man builderMan() {
            buildHead();
            buildBody();
            buildFoot();
            return man;
        }
    }

    测试：
    public class Test{
        public static void main(String[] args) {
            ManBuilder builder = new ManBuilder();
            Man man = builder.builderMan();
        }

5. 