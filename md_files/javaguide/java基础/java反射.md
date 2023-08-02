1. 我的理解:
   1. 静态代理中,我们想要为目标类增加一个新功能,比如日志输出功能,首先,这个目标类要实现一个接口的方法比如shop().对于这个shop方法,我们想要在shop()执行前后,输出日志.这时我们要写一个代理类.这个代理类要包含以下内容:目标类的一个实例,实现了接口的shop()方法,在shop()方法内部增加日志输出的逻辑.如果我们还有其他接口方法比如shop1(),shop2(),...,shop100().如果想要对他们都实现日志输出功能,则需要写100个代理类,这些代理类的特点:目标类的实例各不相同,实现的接口方法各不相同,对于接口方法的增加日志输出功能的逻辑相同.那么,我们不仅仅要写出100个代理类,并且还要重复写100个日志输出功能,并且这个日志输出功能是冗余的.比如后续我想要修改日志输出格式,在输出前加上日期,那么我要修改101次.
   2. 我想要消除这个冗余,能不能日志输出功能只写一次?
   3. 首先观察代理类们的特点:
      1. 代理类要有目标类的实例(因为真正干事的还是目标类,代理类没这个能力,只能依赖它)
      2. 代理类要实现目标类实现的那个接口(客户端想要调用这个shop()方法,代理类必须也要实现这个shop()方法才能起到代理作用)
      3. 代理类实现这个shop()方法并且添加日志输出逻辑:
         1. 首先输出日志:"开始执行shop()"
         2. 执行 目标实例.shop()
         3. 最后输出日志:"执行完毕shop()"
   4. 可以看到,上面的代理类实例是变化的,接口方法shop()也算变化的,而业务逻辑是不变的.如果在一个类中,可以动态的获取代理类实例,动态执行特定的方法,并且然后执行相同的逻辑,那么就能实现日志输出这个代码只写一遍.
   5. java使用动态代理来完成上面的需求,而动态代理又是基于反射实现的.
   6. 对于代理类的特点,java进行了抽象
        1. 代理类需要有目标类的实例的引用. 在java的动态代理方案中,它被放入了InvocationHandler接口的实现类的成员变量中.在这里保存了目标类的引用.而这个InvocationHandler接口的实现类,是动态代理类的一个成员变量
        2. 代理类需要实现目标类实现的那个接口的方法. 在java的动态代理方案中,生成的代理类会对目标类实现的所有的接口方法,在自身均生成一个同名方法,让客户端进行调用
        3. 代理类需要增加一些逻辑代码.代理类通过调用InvocationHandler实现类的invoke方法,此方法包含了业务逻辑,并且能够根据invoke的参数method,动态执行方法

具体看一个动态代理的例子

```java
//目标类的接口
public interface MyInterface {
    void doSomething();
}

//目标类
public class MyTargetClass implements MyInterface {
    @Override
    public void doSomething() {
        System.out.println("Doing something...");
    }
}

//InvocationHandler实现类 这个类必须要写一个,借助它来实现一个逻辑,比如日志输出
public class MyInvocationHandler implements InvocationHandler {
    private MyInterface target;

    public MyInvocationHandler(MyInterface target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before doing something...");
        Object result = method.invoke(target, args);
        System.out.println("After doing something...");
        return result;
    }
}

```
客户端创建代理对象,并且调用doSomething()
```java
MyInterface target = new MyTargetClass();
InvocationHandler handler = new MyInvocationHandler(target);
MyInterface proxy = (MyInterface) Proxy.newProxyInstance(
        target.getClass().getClassLoader(),//目标类
        target.getClass().getInterfaces(),//目标类和代理实现的接口
        handler//代理想要完成的业务逻辑
);
proxy.doSomething();
```

代理类的代码:
```java
public final class $Proxy0 extends Proxy implements MyInterface {
    public $Proxy0(InvocationHandler h) {
        super(h);
    }

    @Override
    public void doSomething() {
        try {
            h.invoke(this, MyInterface.class.getMethod("doSomething"), null);
        } catch (Throwable throwable) {
            throw new UndeclaredThrowableException(throwable);
        }
    }
}
```
这个代理类只有一个方法,是因为 `target.getClass().getInterfaces()`,获取到的这个目标类实现的接口中只有1个方法doSomething,假设它实现了10个接口,并且每个接口都有10个方法,那么这个代理类会有100个方法

这些方法的方法名,就是接口中的方法的方法名.这个方法名必须是一模一样的,因为代理类的使命就是为客户端提供目标类的服务,并且不让客户端直接接触到目标类.所以目标类实现的方法,代理类也要有对应的方法.(它们实现的接口一样)

仔细看代理类doSomething()的代码,可以看到它调用了InvocationHandler的invoke方法.这个InvocationHandler对象是代理类构造方法的第三个参数.

回顾刚刚的invoke方法,有三个参数,proxy,method,args.这里的proxy就是代理类实例,method是Method类型.它通过java的反射获得,method.invoke(args)可以在运行时调用这个方法.
```java
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before doing something...");
        Object result = method.invoke(target, args);
        System.out.println("After doing something...");
        return result;
    }
```

重点关注这里传入的method,使用MyInterface.class.getMethod("doSomething")是获取 MyInterface 接口中名为 "doSomething" 的方法的 Method 对象。
```java
MyInterface.class.getMethod("doSomething")
```
在invoke方法内使用`method.invoke(target, args)`,就相当于target这个实例调用了method这个方法,参数是args
```java
method.invoke(target, args);
```
可以看到,我们没有真正的写任何一个代理类,但是却能通过java获取一个目标类的代理类实例.从头到尾,我们只需要专注于写一个InvocationHandler的实现类,它是用来实现执行业务逻辑的代码.并且只需要写一次

刚刚的例子中,只有一个目标类,并且它只实现了一个接口,并且接口中只有一个方法需要实现.动态代理的优势并不能很好的体现出来.

当有下面这个场景:项目中有10个类,分别实现了10个不同的接口,并且每个接口都有10个方法.我现在想对所有的方法,实现日志输出功能,也就是在方法执行前和执行后输出一句话.借助动态代理,我们只需要写一个InvocationHandler的实现类
重写invoke方法.然后在内部调用method.invoke(target, args),执行目标类的方法的.然后再增加日志输出逻辑
```java
public class MyInvocationHandler implements InvocationHandler {
    private MyInterface target;

    public MyInvocationHandler(MyInterface target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before doing something...");
        Object result = method.invoke(target, args);
        System.out.println("After doing something...");
        return result;
    }
}
```
客户端调用的时候:
这里的MyTargetClass可以被其他类替换,并且能够实现代理功能
```java
MyInterface target = new MyTargetClass();
InvocationHandler handler = new MyInvocationHandler(target);
MyInterface proxy = (MyInterface) Proxy.newProxyInstance(
        target.getClass().getClassLoader(),//目标类
        target.getClass().getInterfaces(),//目标类和代理实现的接口
        handler//代理想要完成的业务逻辑
);
proxy.doSomething();
```

如果使用静态代理:
1. 代码量增加:需要为10个目标类写10个代理类,总共100个方法
2. 接口与代理类代码高耦合:如果接口发生改变(声明的方法发生改变),代理类也要进行改变
3. 业务逻辑代码冗余:10个代理类中的100个代理方法都要写一遍日志输出逻辑.日志输出逻辑是相同的.如果逻辑发生改变,则需要修改100次

总结:java的动态代理机制通过两部分实现:
1. InvocationHandler接口
   1. 它的实现类,在初始化的时候必须要传入一个实例,这个实例就是目标类的实例
   2. 实现`invoke(Object proxy, Method method, Object[] args)`方法,通过`method.invoke(target, args)`可以完成目标类的实例的方法的调用(客户端想要调用的方法)
   3. 在invoke中编写业务逻辑,一个InvocationHandler实现类代表了一个业务逻辑,比如日志输出
2. Proxy类
   1. 使用Proxy.newProxyInstance()方法可以创建一个代理类实例.
   2. 参数1:target.getClass().getClassLoader()目标类的类加载器
   3. 参数2:target.getClass().getInterfaces()目标类的实现的接口列表
   4. 参数3:handler,也就是InvocationHandler接口的实现类的实例
   5. 含义:对target对象,进行代理.实现其实现的所有的接口.并且使用了handler中的业务逻辑.
