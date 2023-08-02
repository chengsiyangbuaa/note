1. 什么是字节码?采用字节码的好处是什么?
   1. 在 Java 中，JVM 可以理解的代码就叫做字节码（即扩展名为 .class 的文件），它不面向任何特定的处理器，只面向虚拟机。Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。所以， Java 程序运行时相对来说还是高效的（不过，和 C++，Rust，Go 等语言还是有一定差距的），而且，由于字节码并不针对一种特定的机器，因此，Java 程序无须重新编译便可在多种不同操作系统的计算机上运行。
   2. 我们需要格外注意的是 .class->机器码 这一步。在这一步 JVM 类加载器首先加载字节码文件，然后通过解释器逐行解释执行，这种方式的执行速度会相对比较慢。而且，有些方法和代码块是经常需要被调用的(也就是所谓的热点代码)，所以后面引进了 JIT（just-in-time compilation） 编译器，而 JIT 属于运行时编译。当 JIT 编译器完成第一次编译后，其会将字节码对应的机器码保存下来，下次可以直接使用。而我们知道，机器码的运行效率肯定是高于 Java 解释器的。这也解释了我们为什么经常会说 Java 是编译与解释共存的语言 。
2. 为什么说 Java 语言“编译与解释并存”？
   1. 这是因为 Java 语言既具有编译型语言的特征，也具有解释型语言的特征。因为 Java 程序要经过先编译，后解释两个步骤，由 Java 编写的程序需要先经过编译步骤，生成字节码（.class 文件），这种字节码必须由 Java 解释器来解释执行。
3. Java 中的几种基本数据类型了解么？
   1. 一共有8种
      1. 数字类型6种:
         1. 整形:byte,short,int,long
         2. 浮点型:float,double
      2. 字符类型1种:
         1. char
      3. 布尔类型1种:
         1. boolean
4. 基本类型和包装类型的区别？
   1. 基本类型不能用于泛型,包装类型可以.
   2. 基本数据类型的局部变量存在虚拟机栈种的局部变量表种,基本数据类型的成员变量（未被 static 修饰 ）存放在 Java 虚拟机的堆中。包装类型属于对象类型，我们知道几乎所有对象实例都存在于堆中。
   3. 占用空间：相比于包装类型（对象类型）， 基本数据类型占用的空间往往非常小。
   4. 默认值：成员变量包装类型不赋值就是 null ，而基本类型有默认值且不是 null。
   5. 比较方式：对于基本数据类型来说，== 比较的是值。对于包装数据类型来说，== 比较的是对象的内存地址。所有整型包装类对象之间值的比较，全部使用 equals() 方法。
5. 包装类型的缓存机制了解么？
   1. Java 基本数据类型的包装类型的大部分都用到了缓存机制来提升性能。
   2. Byte,Short,Integer,Long 这 4 种包装类默认创建了数值 [-128，127] 的相应类型的缓存数据，Character 创建了数值在 [0,127] 范围的缓存数据，Boolean 直接返回 True or False。两种浮点数类型的包装类 Float,Double 并没有实现缓存机制。
   3. valueOf()和自动装箱
      1. 当初始化一个Integer对象的时候,等号右边的值是一个int类型而不是Integer对象的时候,会触发Integer.valueOf()方法,自动装箱int值为一个Integer对象
      2. Integer.valueOf()方法的逻辑是判断int值的范围,如果在默认缓存范围内,则将默认缓存池中的对象直接返回.如果超出范围,则需要new一个新的对象,并且用此数据初始化.
6. 为什么浮点数运算的时候会有精度丢失的风险？
   1. 计算机是二进制的，而且计算机在表示一个数字时，宽度是有限的，无限循环的小数存储在计算机时，只能被截断，所以就会导致小数精度发生损失的情况。这也就是解释了为什么浮点数没有办法用二进制精确表示。
7. 如何解决浮点数运算的精度丢失问题？
   1. BigDecimal 可以实现对浮点数的运算，不会造成精度丢失。通常情况下，大部分需要浮点数精确运算结果的业务场景（比如涉及到钱的场景）都是通过 BigDecimal 来做的。
8. 超过 long 整型的数据应该如何表示？
   1. 在 Java 中，64 位 long 整型是最大的整数类型。
   2. BigInteger 内部使用 int[] 数组来存储任意大小的整形数据。相对于常规整数类型的运算来说，BigInteger 运算的效率会相对较低。
9. 成员变量与局部变量的区别？
   1.  语法形式：从语法形式上看，成员变量是属于类的，而局部变量是在代码块或方法中定义的变量或是方法的参数；成员变量可以被 public,private,static 等修饰符所修饰，而局部变量不能被访问控制修饰符及 static 所修饰；但是，成员变量和局部变量都能被 final 所修饰。
   2.  存储方式：从变量在内存中的存储方式来看，如果成员变量是使用 static 修饰的，那么这个成员变量是属于类的，如果没有使用 static 修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。
   3.  生存时间：从变量在内存中的生存时间上看，成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动生成，随着方法的调用结束而消亡。
   4.  默认值：从变量是否有默认值来看，成员变量如果没有被赋初始值，则会自动以类型的默认值而赋值（一种情况例外:被 final 修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。
10. 静态变量有什么作用？
    1.  静态变量也就是被 static 关键字修饰的变量。它可以被类的所有实例共享，无论一个类创建了多少个对象，它们都共享同一份静态变量。也就是说，静态变量只会被分配一次内存，即使创建多个对象，这样可以节省内存。
    2.  静态变量是通过类名来访问的，例如StaticVariableExample.staticVar（如果被 private关键字修饰就无法这样访问了）。
    3.  通常情况下，静态变量会被 final 关键字修饰成为常量。
11. 字符型常量和字符串常量的区别?
    1.  形式 : 字符常量是单引号引起的一个字符，字符串常量是双引号引起的 0 个或若干个字符。
    2.  含义 : 字符常量相当于一个整型值( ASCII 值),可以参加表达式运算; 字符串常量代表一个地址值(该字符串在内存中存放位置)。
    3.  占内存大小：字符常量只占 2 个字节; 字符串常量占若干个字节。
12. 什么是方法的返回值?方法有哪几种类型？
    1.  无参数无返回值的方法
    2.  有参数无返回值的方法
    3.  有返回值无参数的方法
    4.  有返回值有参数的方法
    5.  构造方法
13. 静态方法为什么不能调用非静态成员?
    1.  静态方法是属于类的，在类加载的时候就会分配内存，可以通过类名直接访问。而非静态成员属于实例对象，只有在对象实例化之后才存在，需要通过类的实例对象去访问。
    2.  在类的非静态成员不存在的时候静态方法就已经存在了，此时调用在内存中还不存在的非静态成员，属于非法操作。
14. 静态方法和实例方法有何不同？
    1.  调用方式:在外部调用静态方法时，可以使用 类名.方法名 的方式，也可以使用 对象.方法名 的方式，而实例方法只有后面这种方式。也就是说，调用静态方法可以无需创建对象 。
    2.  访问类成员是否存在限制:静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），不允许访问实例成员（即实例成员变量和实例方法），而实例方法不存在这个限制。
15. 重载和重写有什么区别？
    1.  地点不同
        1.  重载:同一个类中
        2.  重写:子类对父类
    2.  参数列表
        1.  重载:一定要不同
        2.  重写:一定不能修改
    3.  返回值
        1.  重载:可修改,没有关系
        2.  重写:子类方法返回值类型应比父类方法返回值类型更小或相等(引用类型).基本类型或void,则不可修改
    4.  异常
        1.  重载:可修改
        2.  重写:子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等；
    5.  访问修饰符
        1.  重载:可修改
        2.  重写:一定不能做更严格的限制
    6.  发生阶段
        1.  重载:编译器
        2.  重写:运行期
16. 什么是可变长参数？
    1.  从 Java5 开始，Java 支持定义可变长参数，所谓可变长参数就是允许在调用方法时传入不定长度的参数。就比如下面的这个 printVariable 方法就可以接受 0 个或者多个参数。
    2.  另外，可变参数只能作为函数的最后一个参数，但其前面可以有也可以没有任何其他参数。
17. 遇到方法重载的情况怎么办呢？会优先匹配固定参数还是可变参数的方法呢？
    1.  答案是会优先匹配固定参数的方法，因为固定参数的方法匹配度更高。
18. 面向对象和面向过程的区别
    1.  面向过程把解决问题的过程拆成一个个方法，通过一个个方法的执行解决问题。
    2.  面向对象会先抽象出对象，然后用对象执行方法的方式解决问题。
    3.  面向对象开发的程序一般更易维护、易复用、易扩展。
19. 面向对象三大特征
    1.  封装:封装是指把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。就好像我们看不到挂在墙上的空调的内部的零件信息（也就是属性），但是可以通过遥控器（方法）来控制空调。如果属性不想被外界访问，我们大可不必提供方法给外界访问。但是如果一个类没有提供给外界访问的方法，那么这个类也没有什么意义了。就好像如果没有空调遥控器，那么我们就无法操控空凋制冷，空调本身就没有意义了（当然现在还有很多其他方法 ，这里只是为了举例子）
    2.  继承:不同类型的对象，相互之间经常有一定数量的共同点。例如，小明同学、小红同学、小李同学，都共享学生的特性（班级、学号等）。同时，每一个对象还定义了额外的特性使得他们与众不同。例如小明的数学比较好，小红的性格惹人喜爱；小李的力气比较大。继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高我们的开发效率。
    3.  多态:表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例。
        1.  如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法。
        2.  多态不能调用“只在子类存在但在父类不存在”的方法；
        3.  引用类型变量发出的方法调用的到底是哪个类中的方法，必须在程序运行期间才能确定；
        4.  对象类型和引用类型之间具有继承（类）/实现（接口）的关系；
20. 接口和抽象类有什么共同点和区别？
    1.  共同点
        1.  都不能被实例化。
        2.  都可以包含抽象方法。
        3.  都可以有默认实现的方法（Java 8 可以用 default 关键字在接口中定义默认方法）。
    2.  区别
        1.  接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是所属关系。
        2.  一个类只能继承一个类，但是可以实现多个接口。
        3.  接口中的成员变量只能是 public static final 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。
21. 深拷贝和浅拷贝区别了解吗？什么是引用拷贝？
    1.  浅拷贝：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。
    2.  深拷贝：深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。
22. Object
    1.  ```java
            /**
        * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
        */
        public final native Class<?> getClass()
        /**
        * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
        */
        public native int hashCode()
        /**
        * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
        */
        public boolean equals(Object obj)
        /**
        * native 方法，用于创建并返回当前对象的一份拷贝。
        */
        protected native Object clone() throws CloneNotSupportedException
        /**
        * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
        */
        public String toString()
        /**
        * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
        */
        public final native void notify()
        /**
        * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
        */
        public final native void notifyAll()
        /**
        * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
        */
        public final native void wait(long timeout) throws InterruptedException
        /**
        * 多了 nanos 参数，这个参数表示额外时间（以纳秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 纳秒。。
        */
        public final void wait(long timeout, int nanos) throws InterruptedException
        /**
        * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
        */
        public final void wait() throws InterruptedException
        /**
        * 实例被垃圾回收器回收的时候触发的操作
        */
        protected void finalize() throws Throwable { }
    ```
23. == 和 equals() 的区别
    1.  基本数据类型,没有equals,用==比较多是值
    2.  引用数据类型,==比较内存地址
    3.  .equals()是Object的类,如果比较的类没有重写equals方法,则会使用Object的equals方法,也就是用==比较,即比较内存地址
    4.  如果重写了,就会按照我们写的规则比较
24. hashCode()的作用
    1.  hashCode() 的作用是获取哈希码（int 整数），也称为散列码。这个哈希码的作用是确定该对象在哈希表中的索引位置。
    2.  hashCode() 定义在 JDK 的 Object 类中，这就意味着 Java 中的任何类都包含有 hashCode() 函数。另外需要注意的是：Object 的 hashCode() 方法是本地方法，也就是用 C 语言或 C++ 实现的。
25. 为什么要有 hashCode？
    1.  当你把对象加入 HashSet 时，HashSet 会先计算对象的 hashCode 值来判断对象加入的位置，同时也会与其他已经加入的对象的 hashCode 值作比较，如果没有相符的 hashCode，HashSet 会假设对象没有重复出现。但是如果发现有相同 hashCode 值的对象，这时会调用 equals() 方法来检查 hashCode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。这样我们就大大减少了 equals 的次数，相应就大大提高了执行速度。
    2.  在一些容器（比如 HashMap、HashSet）中，有了 hashCode() 之后，判断元素是否在对应容器中的效率会更高（参考添加元素进HashSet的过程）我们在前面也提到了添加元素进HashSet的过程，如果 HashSet 在对比的时候，同样的 hashCode 有多个对象，它会继续使用 equals() 来判断是否真的相同。也就是说 hashCode 帮助我们大大缩小了查找成本.
    3.  两个对象的hashCode 值相等并不代表两个对象就相等。
26. 为什么重写 equals() 时必须重写 hashCode() 方法？
    1.  因为两个相等的对象的 hashCode 值必须是相等。也就是说如果 equals 方法判断两个对象是相等的，那这两个对象的 hashCode 值也要相等。
    2.  有两种情况
        1.  此类不参与设计哈希的集合的操作（如HashMap、HashSet）,那么它不强制要求必须重写hashCode()方法。
        2.  反之则必须重写hashCode()方法
            1.  比如一个类,重写了equals方法,但是没写hashCode方法,必然会出现:两个实例equals是true,但是hashCode不相同
            2.  此时想要将这两个元素放入hashset集合中,hashset会检查元素的hashcode.由于这两个实例的hashcode不一样,假设它们没有映射到一个桶中,则都会成功存入hashset.然而根据集合的性质,集合是不允许存放两个相同元素的,也就是它们不能equals是true.所以不重写hashcode,会导致这种冲突.
27. String、StringBuffer、StringBuilder 的区别？
    1.  String 是不可变的（后面会详细分析原因）。
    2.  StringBuilder 与 StringBuffer 都继承自 AbstractStringBuilder 类，在 AbstractStringBuilder 中也是使用字符数组保存字符串，不过没有使用 final 和 private 关键字修饰，最关键的是这个 AbstractStringBuilder 类还提供了很多修改字符串的方法比如 append 方法。
    3.  String 中的对象是不可变的，也就可以理解为常量，线程安全。
    4.  AbstractStringBuilder 是 StringBuilder 与 StringBuffer 的公共父类，定义了一些字符串的基本操作，如 expandCapacity、append、insert、indexOf 等公共方法。
    5.  StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。
    6.  StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。
    7.  每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 StringBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。
28. String 为什么是不可变的?
    1.  保存字符串的数组被 final 修饰且为私有的，并且String 类没有提供/暴露修改这个字符串的方法。
    2.  String 类被 final 修饰导致其不能被继承，进而避免了子类破坏 String 不可变。
29. 字符串拼接用“+” 还是 StringBuilder?
    1.  Java 语言本身并不支持运算符重载，“+”和“+=”是专门为 String 类重载过的运算符，也是 Java 中仅有的两个重载过的运算符。
    2.  由于String是不可变的,所以java给出的解决方案是使用+重载后,创建一个StringBuilder,然后调用append方法实现.
    3.  如果大量的String使用+重载,会大量创建StringBuilder对象.
    4.  而改用StringBuilder,就不会出现大量创建StringBuilder对象的现象,而是调用append方法,修改对象本身
30. 字符串常量池的作用了解吗？
    1.  字符串常量池 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。
31. String s1 = new String("abc");这句话创建了几个字符串对象？
    1.  如果字符串常量池中不存在字符串对象“abc”的引用，那么它将首先在字符串常量池中创建，然后在堆空间中创建，因此将创建总共 2 个字符串对象。
    2.  如果字符串常量池中已存在字符串对象“abc”的引用，则只会在堆中创建 1 个字符串对象“abc”。
32. String#intern 方法有什么作用?
    1.  String.intern() 是一个 native（本地）方法，其作用是将指定的字符串对象的引用保存在字符串常量池中，可以简单分为两种情况：
        1.  如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
        2.  如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回。
33. Exception 和 Error 有什么区别？
    1.  在 Java 中，所有的异常都有一个共同的祖先 java.lang 包中的 Throwable 类。Throwable 类有两个重要的子类:
        1.  Exception :程序本身可以处理的异常，可以通过 catch 来进行捕获。Exception 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。
        2.  Error：Error 属于程序无法处理的错误  。例如 Java 虚拟机运行错误（Virtual MachineError）、虚拟机内存不够错误(OutOfMemoryError)、类定义错误（NoClassDefFoundError）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。
34. Checked Exception 和 Unchecked Exception 有什么区别？
    1.  Checked Exception 即 受检查异常 ，Java 代码在编译过程中，如果受检查异常没有被 catch或者throws 关键字处理的话，就没办法通过编译。
    2.   Unchecked Exception 即 不受检查异常 ，Java 代码在编译过程中 ，我们即使不处理不受检查异常也可以正常通过编译。
    3.   RuntimeException 及其子类都统称为非受检查异常，常见的有（建议记下来，日常开发中会经常用到）：
35.  Throwable 类常用方法有哪些？
     1.   String getMessage(): 返回异常发生时的简要描述
     2.   String toString(): 返回异常发生时的详细信息
     3.   String getLocalizedMessage(): 返回异常对象的本地化信息。使用 Throwable 的子类覆盖这个方法，可以生成本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与 getMessage()返回的结果相同
     4.   void printStackTrace(): 在控制台上打印 Throwable 对象封装的异常信息
36.  try-catch-finally 如何使用？
     1.   try块：用于捕获异常。其后可接零个或多个 catch 块，如果没有 catch 块，则必须跟一个 finally 块。
     2.   catch块：用于处理 try 捕获到的异常。
     3.   finally 块：无论是否捕获或处理异常，finally 块里的语句都会被执行。当在 try 块或 catch 块中遇到 return 语句时，finally 语句块将在方法返回之前被执行。
37.  finally 中的代码一定会执行吗？
     1.   不一定的！在某些情况下，finally 中的代码不会被执行。
     2.   就比如说 finally 之前虚拟机被终止运行的话，finally 中的代码就不会被执行。
     3.   程序所在的线程死亡。
     4.   关闭 CPU。
38.  什么是泛型？有什么作用？
     1.   编译器可以对泛型参数进行检测，并且通过泛型参数可以指定传入的对象类型。比如 ArrayList<Person> persons = new ArrayList<Person>() 这行代码就指明了该 ArrayList 对象只能传入 Person 对象，如果传入其他类型的对象就会报错。
39.  项目中哪里用到了泛型？
     1.   自定义接口通用返回结果 CommonResult<T> 通过参数 T 可根据具体的返回类型动态指定结果的数据类型
40.  何为反射？
     1.   如果说大家研究过框架的底层原理或者咱们自己写过框架的话，一定对反射这个概念不陌生。
     2.   反射之所以被称为框架的灵魂，主要是因为它赋予了我们在运行时分析类以及执行类中方法的能力。
     3.   通过反射你可以获取任意一个类的所有属性和方法，你还可以调用这些方法和属性。
41.  反射的应用场景了解么？
     1.   像咱们平时大部分时候都是在写业务代码，很少会接触到直接使用反射机制的场景。
     2.   但是，这并不代表反射没有用。相反，正是因为反射，你才能这么轻松地使用各种框架。像 Spring/Spring Boot、MyBatis 等等框架中都大量使用了反射机制。
     3.   这些框架中也大量使用了动态代理，而动态代理的实现也依赖反射。
42.  java反射的优缺点
     1.   增加程序的灵活性,在运行的过程中动态对类进行修改和操作
     2.   提高代码的复用率,比如动态代理,就是用到了反射来实现
     3.   可以在运行时轻松获得任意一个类的方法\属性,并且还能通过反射进行动态调用
43.  java反射缺点
     1.   反射回涉及到动态类型的解析,所以JVM无法对这些代码进行优化,导致性能要比非反射调用耕地
     2.   使用反射以后,代码的可读性下降
     3.   反射可以绕过一些限制访问的属性或方法,可能会导致破坏了代码本身的抽象性
44.  什么是动态代理
     1.   动态代理是一种设计模式，它在运行时创建并使用代理对象，来实现对真实对象的功能扩展和控制。在 Java 和其他许多编程语言中，动态代理广泛使用。
45.  什么是代理模式 
     1.   在计算机科学中，代理（Proxy）是一种设计模式，它提供了对另一个对象或服务的接口。代理可以接收所有的调用请求，进行一些处理，然后转发给实际的服务对象。代理的目的可以是控制对实际对象的访问，或者添加在调用实际对象之前或之后需要进行的一些额外操作。
     2.   代理可以控制对原始对象的访问,让客户端只能访问到代理.
46.  动态代理和静态代理的区别
     1.   动态代理和静态代理是在软件开发中常见的两种代理模式。它们主要的区别在于代理类的生成时间和方式。
     2.   静态代理：在程序运行前，代理类的 .java 文件就已经存在，.class 文件也已经被编译。静态代理需要为每一个被代理的类或接口都创建一个代理类，这会导致工作量大、不易管理，同时，一旦代理对象的方法发生改变，代理类也必须要进行相应的修改。
     3.   静态代理需要一个接口,代理类,目标类.但是如果要是有很多的接口,那就会产生很多的代理类.由于代理类是运行之前就写好了的java类,所以无法在运行时动态的选择要代理的目标类,为了解决这个问题,使用了动态代理
     4.   动态代理：动态代理的代理类不是在程序运行前创建的，而是在运行时根据需要动态生成的。这样，我们就可以使用一种通用的代理类来代理任何类型的对象，这大大减少了代码量，提高了工作效率。在Java中，可以使用 java.lang.reflect.Proxy 类和 java.lang.reflect.InvocationHandler 接口来实现动态代理。
     5.   总结:使用动态代理可以大大减少代码量,并且减少代码冗余.
47.  程序为什莫需要代理?
     1.   对象如果觉得身上干的事太多,可以通过代理来转移部分职责
     2.   通过代理处理被代理非核心的工作,解耦,提高维护
     3.   无序干涉被代理者的情况,代理对目标进行增强的作用
48.  动态代理和反射的关系:
     1.   反射 是Java中的一个重要特性，它使得Java程序具有在运行时自我检查的能力，它可以实现在运行时访问、检测和修改类的属性，方法等信息，并且能够调用方法和创建对象实例。这提供了极大的灵活性和动态性。
     2.   动态代理 则是运行时动态地生成代理对象，这个代理对象可以在调用实际对象的方法前后添加一些自定义的操作。
     3.   动态代理的实现就是依赖于反射机制，通过反射获取实际对象的类型信息，然后动态生成一个代理对象，并且使用 java.lang.reflect.InvocationHandler 的 invoke 方法来拦截对实际对象方法的调用，并在其中插入代理逻辑。
     4.   在java.lang.reflect.Proxy类中，newProxyInstance方法就是用于创建动态代理对象的。它接收一个类加载器、一组接口以及一个实现了InvocationHandler接口的实例，返回一个实现了给定接口且由给定的InvocationHandler处理方法调用的代理实例。
     5.   我的理解:
          1.   静态代理中,我们想要为目标类增加一个新功能,比如日志输出功能,首先,这个目标类要实现一个接口的方法比如shop().对于这个shop方法,我们想要在shop()执行前后,输出日志.这时我们要写一个代理类.这个代理类要包含以下内容:目标类的一个实例,实现了接口的shop()方法,在shop()方法内部增加日志输出的逻辑.如果我们还有其他接口方法比如shop1(),shop2(),...,shop100().如果想要对他们都实现日志输出功能,则需要写100个代理类,这些代理类的特点:目标类的实例各不相同,实现的接口方法各不相同,对于接口方法的增加日志输出功能的逻辑相同.那么,我们不仅仅要写出100个代理类,并且还要重复写100个日志输出功能,并且这个日志输出功能是冗余的.比如后续我想要修改日志输出格式,在输出前加上日期,那么我要修改101次.
          2.   我想要消除这个冗余,能不能日志输出功能只写一次?
          3.   首先观察代理类们的特点:
               1.   代理类要有目标类的实例(因为真正干事的还是目标类,代理类没这个能力,只能依赖它)
               2.   代理类要实现目标类实现的那个接口(客户端想要调用这个shop()方法,代理类必须也要实现这个shop()方法才能起到代理作用)
               3.   代理类实现这个shop()方法并且添加日志输出逻辑:
                    1.   首先输出日志:"开始执行shop()"
                    2.   执行 目标实例.shop()
                    3.   最后输出日志:"执行完毕shop()"
          4. 可以看到,如果能够获得目标类的实例,并且获取目标类实现的接口(的方法)
          5. java使用动态代理来完成上面的需求,而动态代理又是基于反射实现的.
          6. 对于代理类的特点,java进行了抽象
             1. 代理类需要有目标类的实例(静态代理)的引用,在java的动态代理方案中,它被放入了InvocationHandler接口的实现类的成员变量中.在这里保存了目标类的引用.而这个InvocationHandler接口的实现类,是代理类的一个成员变量
             2. 代理类需要实现目标类实现的那个接口的方法,在java的动态代理方案中,生成的代理类会对目标类实现的所有的接口方法,在自身均生成一个同名方法,让客户端进行调用
             3. 代理类通过调用InvocationHandler实现类的invoke方法,此方法包含了日志输出的逻辑,并且能够根据invoke的参数method,执行不同的方法
          7. 展示一个动态代理的例子
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
49. 谈谈反射机制的优缺点
    1.  优点：可以让咱们的代码更加灵活、为各种框架提供开箱即用的功能提供了便利
    2.  缺点：让我们在运行时有了分析操作类的能力，这同样也增加了安全问题。比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）。另外，反射的性能也要稍差点，不过，对于框架来说实际是影响不大的。
50. 值传递和引用传递
    1. 值传递：方法接收的是实参值的拷贝，会创建副本。
    2. 引用传递：方法接收的直接是实参所引用的对象在堆中的地址，不会创建副本，对形参的修改将影响到实参。
51. 为什莫java只有值传递而不是引用传递
    1.  引用传递:形参的修改相会影响到实参
    2.  值传递:形参的修改不会影响到实参
52. 什么是 Java 序列化
    1.  序列化是把对象改成可以存到磁盘或通过网络发送到其他运行中的 Java 虚拟机的二进制格式的过程, 并可以通过反序列化恢复对象状态. Java 序列化API给开发人员提供了一个标准机制, 通过 java.io.Serializable 和 java.io.Externalizable 接口, ObjectInputStream 及ObjectOutputStream 处理对象序列化. Java 程序员可自由选择基于类结构的标准序列化或是他们自定义的二进制格式, 通常认为后者才是最佳实践, 因为序列化的二进制文件格式成为类输出 API的一部分, 可能破坏 Java 中私有和包可见的属性的封装.
    2.  常见的场景:
        1.  对象在进行网络传输（比如远程方法调用 RPC 的时候）之前需要先被序列化，接收到序列化的对象之后需要再进行反序列化；
        2.  将对象存储到文件之前需要进行序列化，将对象从文件中读取出来需要进行反序列化；
        3.  将对象存储到数据库（如 Redis）之前需要用到序列化，将对象从缓存数据库中读取出来需要反序列化；
        4.  将对象存储到内存之前需要进行序列化，从内存中读取出来之后需要进行反序列化。
53. 序列化协议对应于 TCP/IP 4 层模型的哪一层？
    1.  序列化协议属于 TCP/IP 协议应用层的一部分
54. JDK 自带的序列化方式
    1.  实现java.io.Serializable
    2.  serialVersionUID 有什么作用？
        1.  序列化号 serialVersionUID 属于版本控制的作用。反序列化时，会检查 serialVersionUID 是否和当前类的 serialVersionUID 一致。如果 serialVersionUID 不一致则会抛出 InvalidClassException 异常。强烈推荐每个序列化类都手动指定其 serialVersionUID，如果不手动指定，那么编译器会动态生成默认的 serialVersionUID。
    3.  serialVersionUID 不是被 static 变量修饰了吗？为什么还会被“序列化”？
        1.  static 修饰的变量是静态变量，位于方法区，本身是不会被序列化的。 static 变量是属于类的而不是对象。你反序列之后，static 变量的值就像是默认赋予给了对象一样，看着就像是 static 变量被序列化，实际只是假象罢了。
        2.  官方说明:如果想显式指定 serialVersionUID ，则需要在类中使用 static 和 final 关键字来修饰一个 long 类型的变量，变量名字必须为 "serialVersionUID" 。也就是说，serialVersionUID 只是用来被 JVM 识别，实际并没有被序列化。
    4.  如果有些字段不想进行序列化怎么办？
        1.  对于不想进行序列化的变量，可以使用 transient 关键字修饰。
        2.  transient 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 transient 修饰的变量值不会被持久化和恢复。
        3.  transient 只能修饰变量，不能修饰类和方法。
        4.  transient 修饰的变量，在反序列化后变量值将会被置成类型的默认值。例如，如果是修饰 int 类型，那么反序列后结果就是 0。
        5.  static 变量因为不属于任何对象(Object)，所以无论有没有 transient 关键字修饰，均不会被序列化。
55. 说说你对 BigDecimal 的理解？
    1.  ava中进行浮点数运算的时候，会出现丢失精度的问题。那么我们如果在进行商品价格计算的时候，就会出现问题。很有可能造成我们手中有0.06元，却无法购买一个0.05元和一个0.01元的商品。使用Java中的BigDecimal类来解决这类问题。
    2.  Java中float的精度为6-7位有效数字。double的精度为15-16位。
    3.  防止精度丢失,使用string参数的构造方法创建对象.或者只用BigDecimal.valueOf(double val) 静态方法来创建对象(内部实际执行了Double的toString方法,Double的toString方法按照bouble的实际能表达的精度对尾数进行了截断)。
    4.  进行货币计算的时候,还可以使用整数作为最小单位,比如以分为单位,这样可以避免浮点运算中的精度问题
56. 