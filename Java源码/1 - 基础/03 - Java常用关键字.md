## static

### 修饰的对象

static 只能修饰类变量、方法、方法块

当 static 修饰类变量时，如果该变量是 public 的话，表示该变量任何类都可以直接访问，而且无需初始化类，直接使用`类名.static变量名`这种形式访问。

多个线程同时对共享变量进行读写时，很容易出现线程安全问题。解决方法如下所示：

1. 线程不安全的集合类换成线程安全的集合类
2. 手动加锁

当 static 修饰方法时，如果该方法是 public 的话，表示该方法和当前类的实例是无关的，类和类的任意实力都可以直接访问。

静态方法中代码只能使用静态方法，不能调用普通方法。static 方法内部的变量在执行时时没有线程安全问题的。方法执行时，数据运行在栈里面，栈的数据每个线程都是隔离的。

当 static 修饰方法块时，被称为静态代码块，常常用于类启动之前，初始化一些值。静态代码块只能调用静态变量，并且静态变量需要写在静态代码块前面，不然编译会报错。

### 初始化时机

静态代码块只会在类初始化的时候执行一次。

当同时存在子类和父类时，代码执行顺序为：`父类静态 => 子类静态 => 父类构造代码块 => 父类构造方法 => 子类构造代码块 => 子类构造方法`

1. 静态变量初始化在静态代码块初始化之前执行
2. 静态变量和静态代码块在类构造器之前执行
3. 父类的静态变量和静态代码块在子类初始化之前执行

## final

1. 被 final 修饰的类，无法继承
2. 被 final 修饰的类，类的方法无法覆盖
3. 被 final 修饰的变量，变量在声明的时候，就必须完成初始化，而且以后不能修改其内存地址。对于 List、Map这些集合类说，被final修饰后，是可以修改其内部值的，但是却无法修改其初始化时的内存地址。

```java
final String str = new String();
// 编译报错，str被final修饰，指向的地址不可修改
str = new String("123");

final List list = new ArrayList<>();
// 可以修改list指向集合的数据
list.add(1);
list.add(2);
// 不能修改list存放的地址
list = new ArrayList();
```

## try、catch、finally

这三个关键字常用于捕捉异常的一套流程，try 用来确认代码执行的范围，catch 捕捉可能会发生的异常，finally 用来执行程序在最后一定要执行的代码块。

1. 代码的执行顺序为：`try => catch => finally`
2. finally 先执行后，再抛出 catch 的异常
3. 最终捕获的异常是 catch 的异常，try 抛出来的异常被 catch 处理掉了。如果遇到 catch 也有可能抛出异常时，我们可以先打印出 try 的异常，这样 try 的异常在日志中就会有所体现。

## volatile

在多核 CPU 下，为了提高效率，线程在取值的时候，是直接读取的 CPU 缓存，而不是内存。CPU 缓存会比内存更快。线程从CPU缓存获取值的时候，如果不存在，就会从内存中加载。

CPU缓存中的值和内存中的值可能并不是时刻都同步，导致线程计算的值可能不是最新的，共享变量的值可能被其他线程修改了，此时修改的是机器内存的值。CPU缓存中的值是旧的。

这时候有个机制，内存会主动通知CPU缓存，当前共享变量的值已经失效了，CPU缓存会从内存中重新加载一份最新的。

volatile 关键字触发这种机制，加了 volatile 的关键字的变量，就会被识别成共享变量，内存中值被修改后，通知各个CPU缓存，使CPU缓存中的值更新。从而保证线程从CPU缓存中拿出来的是最新的。

## transient

修饰的对象无需序列化

## default

接口的默认方法，子类无需强制实现，但自己必须有默认的实现。

## 面试题

一、很多变量和方法被 static 和 final 两个关键字修饰，为什么这么做

1. 变量和方法跟类的实例无关，可以直接使用
2. 强调变量内存地址不可变，方法不可继承覆写，强调方法内部的稳定性

二、catch 中发生了未知异常，finally 还会执行吗

会，finally执行完成以后，才会抛出catch中的异常
