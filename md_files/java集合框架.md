# 集合框架由浅入深
集合框架可以分为两大类,Collection和Map.
对于集合的理解,可以是存储一堆元素的集合.
Collection相对Map简单,它只存储这些元素本身.
Map不仅存这些元素,还要存这些元素对应的键key.这里元素理解为值value
本质上两类集合都是用来存储大量元素和设计出来的结构.
(图源:https://github.com/Snailclimb/JavaGuide/blob/main/docs/java/collection/java-collection-questions-01.md)
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230713194510.png)
## 初入集合框架

掌握了ArrayList和LinkedList的一些基本使用方法，能够利用他们做一些基础的算法题。但是java是如何实现这些数据结构的呢？本文一点一点深入理解java集合框架。首先从LinkedList为切入点,由底向上分析java集合框架.快速分析其他集合类.

### LinkedList源码分析
进入LinkedList的源码，可以看到LinkedList的继承和实现关系。并且依次深入阅读源码理解java的集合框架.
```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
```
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230713172448.png)
下面从这些继承实现关系入手一点一点摸索java的集合框架。
#### List<E\>接口
```java
public interface List<E> extends Collection<E> 
```
可以看到`List`接口继承了`Collection`
除此之外,`Set`接口也继承了`Collection`
由此可知`List`和`Set`是并列的关系

#### Collection<E\>接口
```java
public interface Collection<E> extends Iterable<E> 
```
`Collection`接口继承了`Iterable`接口
下图是`Collection`定义的方法.这些方法用来对集合进行抽象.
java中的集合应该有图中的这些功能:添加元素,清除元素,(是否)包含元素,判断集合是否为空,移除元素,返回集合大小,集合转数组等功能.
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230713175632.png)

#### Iterable<E\>接口
可以看到`Iterable`接口不再继承任何接口.
```
public interface Iterable<T>
```
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230713174303.png)

`Iterable`接口简单介绍

---

`Iterable`接口是Java中的一个接口，位于`java.lang`包中。它是Java集合框架的基础接口之一，用于表示一组元素的集合。`Iterable`接口定义了一个抽象方法`iterator()`，该方法返回一个实现了`Iterator`接口的对象。

`Iterable`接口的主要目的是提供一种统一的方式来迭代（遍历）集合中的元素。通过实现`Iterable`接口，可以使集合类能够使用增强的for循环来遍历元素。


#### 分析List接口
回顾List接口的继承关系.
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230713173719.png)
阅读源码可以看到`List<E>`接口定义了这些方法。
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230713175923.png)
在集合的基础上,List是一种能实现更多功能的更强大的集合.
具体来看一看它比Collection强大在何处:
1. add(int,E) 可以按照index向集合插入元素
2. get(int) 可以按照index获取元素 (Collection不能根据index获取元素,集合没有获取元素的功能)
3. indexOf(Object) 可以获得元素在集合中的index
4. remove(index) 可以根据index移除元素
5. set(int,E) 可以根据index设置相应位置的元素.
6. sort 排序
7. subList(int,int) 获取子集合

最后阅读源码中的注释可以了解到集合的特性:
1. 有序
2. 可用index访问
3. 可重复
`An ordered collection (also known as a sequence). The user of this interface has precise control over where in the list each element is inserted. The user can access elements by their integer index (position in the list), and search for elements in the list.Unlike sets, lists typically allow duplicate elements.`


#### 分析Set接口
可以看到`Set`接口继承了`Collection`接口.
```java
public interface Set<E> extends Collection<E>
```
下图给出了`Set`接口的方法.可以看到从方法层面,Set接口并没有提供一些新的方法.
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230713181458.png)

在源码注释中可以找出`Set`的一些特性
 `A collection that contains no duplicate elements. `
由此可见`Set`的最大的特性就是:**不重复**.

#### (补充)分析Queue接口
`Queue`接口也直接继承自`Collection`接口
```
public interface Queue<E> extends Collection<E>
```
从源码注释可以得知`Queue`的特性:
 * 一个用于保存元素并支持插入、提取和检查操作的接口。
 * 队列通常按照先进先出（FIFO）的顺序对元素进行处理。
 * 除了基本的集合操作外，队列还提供了额外的插入、提取和检查操作。
`A collection designed for holding elements prior to processing. Besides basic Collection operations, queues provide additional insertion, extraction, and inspection operations. Each of these methods exists in two forms: one throws an exception if the operation fails, the other returns a special value (either null or false, depending on the operation). The latter form of the insert operation is designed specifically for use with capacity-restricted Queue implementations; in most implementations, insert operations cannot fail. `

这是`Queue`定义的方法.
![](https://tallestdaisy.oss-cn-beijing.aliyuncs.com/20230713184640.png)

#### (补充)分析Deque接口
`Deque`接口直接继承自`Queue`接口
```java
public interface Deque<E> extends Queue<E>
```
从源码注释可以得知`Deque`的特性
* 双端队列（Double Ended Queue）接口，扩展了`Queue`接口，支持在两端进行插入、提取和检查操作。
 * `Deque`接口既支持先进先出（FIFO）的操作，也支持后进先出（LIFO）的操作，因此可以作为队列和栈的替代品。
`A linear collection that supports element insertion and removal at both ends. The name deque is short for "double ended queue" and is usually pronounced "deck". Most Deque implementations place no fixed limits on the number of elements they may contain, but this interface supports capacity-restricted deques as well as those with no fixed size limit.`


#### 分析LinkedList
现在回到最初的LinkedList.重新看LinkedList的继承实现关系:
```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
```
我们已经能够读懂`implements List<E>`的意义了.
1. `LinkedList`实现了集合`Collection`接口,它能够完成`Collection`的一些基本功能:添加元素,清除元素,(是否)包含元素,判断集合是否为空,移除元素,返回集合大小,集合转数组等功能.
2. `LinkedList`实现了`List<E>`接口和`Queue<E>`,它有以下特性:
   1. 有序,保留了元素的进入顺序
   2. 访问,可以使用index对元素进行访问
   3. 可重复,元素可以重复
   4. 是一个队列,同时也是一个双端队列.
#### 总结
以上是从集合角度对`LinkedList`进行的分析.
java中的集合框架的顶层接口就是`Collection`接口.`Collection`接口将集合应该有的功能抽象出来:添加元素,清除元素,(是否)包含元素,判断集合是否为空,移除元素,返回集合大小,集合转数组等功能.它是最抽象的接口.
`Collection`下面紧接着被两个接口继承:`List`,`Set`和`Queue`.这几个接口也是很抽象的接口.但是相比较`Collection`它们拥有独特的性质和意义.
其中`List`有以下特性:
1. 有序,保留了元素的进入顺序
2. 访问,可以使用index对元素进行访问
3. 可重复,元素可以重复

`Set`有以下特性:
1. 不重复
2. 无序

`Queue`有以下特性:
1. 一个用于保存元素并支持插入、提取和检查操作的接口。
2. 队列通常按照先进先出（FIFO）的顺序对元素进行处理。
3. 除了基本的集合操作外，队列还提供了额外的插入、提取和检查操作。

### ArrayList分析
可以看到`ArrayList`实现了`List`接口
```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

根据已知的集合框架的知识,ArrayList具有List的特性,也就是:
1. 有序,保留了元素的进入顺序
2. 访问,可以使用index对元素进行访问
3. 可重复,元素可以重复

同时看源码注释可以得知ArrayList的其他特性
1. 可以扩容
`Resizable-array implementation of the List interface. `

### Vector分析
可以看到`Vector`实现了`List`接口
```java
public class Vector<E>
    extends AbstractList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```
根据已知的集合框架的知识,`Vector`具有`List`的特性,也就是:
1. 有序,保留了元素的进入顺序
2. 访问,可以使用`index`对元素进行访问
3. 可重复,元素可以重复

同时看源码注释可以得知ArrayList的其他特性
1. 适用于需要多线程操作或者需要线程安全性的场景，同时具备动态调整大小的能力。
2. 然而，由于同步性的处理，`Vector`的性能相对较低，因此在无需线程安全性的场景下，推荐使用`ArrayList`作为替代。
`Unlike the new collection implementations, Vector is synchronized. If a thread-safe implementation is not needed, it is recommended to use ArrayList in place of Vector.`


### Stack分析
可以看到`Stack`继承自`Vector`.
```java
class Stack<E> extends Vector<E> 
```
根据已有的知识,stack有Vector的特性

同时,阅读源码注释可以得知Stack的其他特性:
1. Stack类表示一个后进先出（LIFO）的对象堆栈
2. Deque接口及其实现提供了一个更完整和一致的LIFO堆栈操作集，应优先使用它们，而不是使用此类,例如` Deque<Integer> stack = new ArrayDeque<Integer>();`

` The Stack class represents a last-in-first-out (LIFO) stack of objects. It extends class Vector with five operations that allow a vector to be treated as a stack. The usual push and pop operations are provided, as well as a method to peek at the top item on the stack, a method to test for whether the stack is empty, and a method to search the stack for an item and discover how far it is from the top.
When a stack is first created, it contains no items.
A more complete and consistent set of LIFO stack operations is provided by the Deque interface and its implementations, which should be used in preference to this class. For example:Deque<Integer> stack = new ArrayDeque<Integer>(); `

### ArrayDeque分析
可以看到`ArrayDeque`实现了Deque接口.
```java
public class ArrayDeque<E> extends AbstractCollection<E>
                           implements Deque<E>, Cloneable, Serializable
```
根据已有的知识,ArrayDeque有Deque的特性,即:
* 双端队列（Double Ended Queue）接口，扩展了`Queue`接口，支持在两端进行插入、提取和检查操作。
 * `Deque`接口既支持先进先出（FIFO）的操作，也支持后进先出（LIFO）的操作，因此可以作为队列和栈的替代品。

同时,阅读源码注释可以得知Stack的其他特性:
1. 动态数组,用动态数组实现,长度可以动态变化
2. 线程不安全
3. 如果所在的环境线程安全:
   1. 被当作栈使用的时候,比`Stack`效率高
   2. 被当作队列使用的时候,比`LinkedList`效率高.


`Resizable-array implementation of the Deque interface. Array deques have no capacity restrictions; they grow as necessary to support usage. They are not thread-safe; in the absence of external synchronization, they do not support concurrent access by multiple threads. Null elements are prohibited. This class is likely to be faster than Stack when used as a stack, and faster than LinkedList when used as a queue.`


