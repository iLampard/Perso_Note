# Java OOP Snippets


### Interface

#### Access specifier of methods in interfaces
所有接口的方法默认是public，数据成员是public static final(所以必须初始化)。
```java
interface Test { 
  int x = 10;  // x is public static final and must be initialized here 
  void foo();  // foo() is public 
} 
```

#### 接口的特点
- 接口不能实例化，但是可以被子类实例化。
- 接口的子类要么是抽象类，要么重写接口中的所有抽象方法。



### Abstract class
#### 抽象类与抽象方法
- 抽象类不能实例化，但是可以被子类实例化。
```java
abstract class Base { 
    abstract void fun(); 
} 

class Derived extends Base { 
    void fun() { System.out.println("Derived fun() called"); } 
} 

class Main { 
    public static void main(String args[]) {  
      
        // Uncommenting the following line will cause compiler error as the  
        // line tries to create an instance of abstract class. 
        // Base b = new Base(); 
  
        // We can have references of Base type. 
        Base b = new Derived(); 
        b.fun();  
    } 
} 

```
Output:
```
Derived fun() called
```

- 抽象类可以有构造函数，当子类实例化的时候被调用。
- 抽象类不一定有抽象方法，有抽象方法的类一定是抽象类。
- 抽象类的子类要么是抽象类，要么重写抽象类中的所有抽象方法。

本文参考了[Abstract Classes in Java](https://www.geeksforgeeks.org/abstract-classes-in-java/)。


### 内部类
- 内部类可以直接访问外部类的成员，包括私有成员。
- 外部类要访问内部类的成员，必须创建对象。

### Package

#### 带包的类的编译和运行

编译的时候带上'-d'即可。
```java
javac -d HelloWorld.java
java HelloWorld
```



#### 注意事项
- package 语句必须是程序的第一条可执行代码。
- package 语句在一个java文件中只能有一个。
- 如果没有package, 默认表示无包名。

#### 权限修饰符
- 用public修饰的类可在本类、同包、子类、其他包中互相访问。
- 用protected修饰的类在本类、同包、子类中互相访问， 但不可以在包外没有继承关系的类中互相访问。
- default修饰的类在本类、同包中可以互相访问，但不可以在包外(不管有没有继承关系)的类中互相访问。
- 用private修饰的类只能在本类中访问。
- private同protected均不可以修饰类

| | public | protected| default | private |
|----- |----- |-----|---- | -----|
|同一类中| ok | ok | ok | ok |
|同一包子类、其他类| ok | ok | ok |  |
|不同包子类| ok | ok |  |  |
|不同包其他类| ok | |  |  |