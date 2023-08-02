1. 简单介绍以下java集合?
   1. Java 集合， 也叫作容器，主要是由两大接口派生而来：一个是 Collection接口，主要用于存放单一元素；另一个是 Map 接口，主要用于存放键值对。对于Collection 接口，下面又有三个主要的子接口：List、Set 和 Queue。
2. 说说 List, Set, Queue, Map 四者的区别？
   1. List, Set, Queue, Map都是接口,它们属于java集合的组成部分.
   2. List,Set,Queue三个接口都继承自Collection接口,所以这些集合存储的元素都是单一元素.而Map接口是专门负责存储key-value键值对的容器.
   3. 下面分别说说这四个接口
      1. List接口中,存储的元素是有序的,可重复的
      2. Set接口,是对数学中的集合的抽象,所以内部的元素是无序的并且不可重复
      3. Queue接口是基于FIFO的队列规则的集合,存储的元素也是有序的可重复的
      4. Map接口专门存储键值对.
3. 说一说集合框架底层数据结构
   1. List
      1. ArrayList:Object[]数组
      2. Vector:Object[]数组
      3. LinkedList:双向链表
   2. Set
      1. HashSet(无序):基于HashMap实现,底层采用HashMap保存元素
      2. LinkedHashSet:是HashSet的子类
      3. TreeSet(有序):红黑树
   3. Queue
      1. PriorityQueue: Object[] 数组来实现二叉堆
      2. ArrayQueue: Object[] 数组 + 双指针
   4. Map
      1. HashMap:JDK1.8 之前 HashMap 由数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）。JDK1.8 以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间
      2. LinkedHashMap：LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。
      3. Hashtable：数组+链表组成的，数组是 Hashtable 的主体，链表则是主要为了解决哈希冲突而存在的
      4. TreeMap：红黑树（自平衡的排序二叉树）
4. ArrayList 和 Array（数组）的区别？
   1. ArrayList会根据实际存储的元素动态地扩容或缩容，而 Array 被创建之后就不能改变它的长度了。
   2. ArrayList 允许你使用泛型来确保类型安全，Array 则不可以。
   3. ArrayList 中只能存储对象。对于基本类型数据，需要使用其对应的包装类（如 Integer、Double 等）。Array 可以直接存储基本类型数据，也可以存储对象。
   4. 支持插入、删除、遍历等常见操作，并且提供了丰富的 API 操作方法，比如 add()、remove()等。Array 只是一个固定长度的数组，只能按照下标访问其中的元素，不具备动态添加、删除元素的能力。
   5. ArrayList创建时不需要指定大小，而Array创建时必须指定大小。
5. ArrayList 和 Vector 的区别?（了解即可）
   1. ArrayList 是 List 的主要实现类，底层使用 Object[]存储，适用于频繁的查找工作，线程不安全 。
   2. Vector 是 List 的古老实现类，底层使用Object[] 存储，线程安全。
6. Vector 和 Stack 的区别?（了解即可）
   1. Vector 和 Stack 两者都是线程安全的，都是使用 synchronized 关键字进行同步处理。
   2. Stack 继承自 Vector，是一个后进先出的栈，而 Vector 是一个列表。
7. ArrayList 可以添加 null 值吗？
   1. ArrayList 中可以存储任何类型的对象，包括 null 值。不过，不建议向ArrayList 中添加 null 值， null 值无意义，会让代码难以维护比如忘记做判空处理就会导致空指针异常。
8. ArrayList 插入和删除元素的时间复杂度？
   1. 插入
      1. 头部插入:由于需要将所有元素都依次向后移动一个位置，因此时间复杂度是 O(n)。
      2. 尾部插入:
         1. 当 ArrayList 的容量未达到极限时，往列表末尾插入元素的时间复杂度是 O(1)，因为它只需要在数组末尾添加一个元素即可；
         2. 当容量已达到极限并且需要扩容时，则需要执行一次 O(n) 的操作将原数组复制到新的更大的数组中，然后再执行 O(1) 的操作添加元素。
      3. 指定位置插入：需要将目标位置之后的所有元素都向后移动一个位置，然后再把新元素放入指定位置。这个过程需要移动平均 n/2 个元素，因此时间复杂度为 O(n)。
   2. 删除
      1. 头部删除：由于需要将所有元素依次向前移动一个位置，因此时间复杂度是 O(n)。
      2. 尾部删除：当删除的元素位于列表末尾时，时间复杂度为 O(1)。
      3. 指定位置删除：需要将目标元素之后的所有元素向前移动一个位置以填补被删除的空白位置，因此需要移动平均 n/2 个元素，时间复杂度为 O(n)。
9. LinkedList 插入和删除元素的时间复杂度？
   1.  头部插入/删除：只需要修改头结点的指针即可完成插入/删除操作，因此时间复杂度为 O(1)。
   2.  尾部插入/删除：只需要修改尾结点的指针即可完成插入/删除操作，因此时间复杂度为 O(1)。
   3.  指定位置插入/删除：需要先移动到指定位置，再修改指定节点的指针完成插入/删除，因此需要移动平均 n/2 个元素，时间复杂度为 O(n)。
10. LinkedList 为什么不能实现 RandomAccess 接口？
    1.  RandomAccess 是一个标记接口，用来表明实现该接口的类支持随机访问（即可以通过索引快速访问元素）。由于 LinkedList 底层数据结构是链表，内存地址不连续，只能通过指针来定位，不支持随机快速访问，所以不能实现 RandomAccess 接口。
11. ArrayList 与 LinkedList 区别?
    1.  是否保证线程安全： ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全；
    2.  底层数据结构： ArrayList 底层使用的是 Object 数组；LinkedList 底层使用的是 双向链表 数据结构（JDK1.6 之前为循环链表，JDK1.7 取消了循环。注意双向链表和双向循环链表的区别，下面有介绍到！）
    3.  插入和删除是否受元素位置的影响：
        1.  ArrayList 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。 比如：执行add(E e)方法的时候， ArrayList 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是 O(1)。但是如果要在指定位置 i 插入和删除元素的话（add(int index, E element)），时间复杂度就为 O(n)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。
        2.  LinkedList 采用链表存储，所以在头尾插入或者删除元素不受元素位置的影响（add(E e)、addFirst(E e)、addLast(E e)、removeFirst()、 removeLast()），时间复杂度为 O(1)，如果是要在指定位置 i 插入和删除元素的话（add(int index, E element)，remove(Object o),remove(int index)）， 时间复杂度为 O(n) ，因为需要先移动到指定位置再插入和删除。
    4.  是否支持快速随机访问： LinkedList 不支持高效的随机元素访问，而 ArrayList（实现了 RandomAccess 接口） 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于get(int index)方法)。
    5.  内存空间占用： ArrayList 的空间浪费主要体现在在 list 列表的结尾会预留一定的容量空间，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为要存放直接后继和直接前驱以及数据）。
12. 说一说 ArrayList 的扩容机制吧
    1.  ArrayList的构造方法有三种:
        1.  无参,会将一个空列表赋值给它,并且初始容量为10
        2.  int,初始容量用户指定
        3.  Collection
    2.  扩容机制以add方法举例
        1.  在添加元素前,要确保容器的容量充足,首先调用ensureCapacityInternal(size + 1),意为确保添加元素后,容量至少为size+1
        2.  ensureCapacityInternal(int minCapacity)中的形参minCapacity,为size+1,意为最小要保证的容量值
        3.  当 要 add 进第 1 个元素时，minCapacity 为 1，在 Math.max()方法比较后，minCapacity 为 10。(只有无参构造放啊创建的实例,在第一次add元素时会这样比较)
        4.  然后必然会执行ensureExplicitCapacity()
        5.  ensureExplicitCapacity(int minCapacity)方法需要判断是否需要扩容.minCapacity - elementData.length > 0如果最小要保证的容量之大于内置数组的长度(也就是当前容量),就需要扩容,也就是执行grow()方法
        6.  grow(int minCapacity)是ArrayList扩容的核心方法
            1.  具体的扩容是先获取扩容后的容量int newCapacity = oldCapacity + (oldCapacity >> 1);扩容后的容量是旧容量的1.5倍,具体用位运算实现
            2.  这时候获取的扩容后的容量,是根据旧容量计算得来的.还需要与最小需求的容量值进行比较.如果计算得来的容量值比最小需求的容量之小,则将最小需求的容量之设为新的容量值(这个比较是必要的,因为如果只add一个元素,计算得到的容量15肯定比11大,但是如果add了一个长度为10的集合,那么15就小于20,则需要将新容量设为20)
            3.  最后通过Arrays.copyOf(elementData, newCapacity);进行扩容
            4.  System.arraycopy() 和 Arrays.copyOf()方法
                1.  看两者源代码可以发现 copyOf()内部实际调用了 System.arraycopy() 方法
                2.  arraycopy() 需要目标数组，将原数组拷贝到你自己定义的数组里或者原数组，而且可以选择拷贝的起点和长度以及放入新数组中的位置 copyOf() 是系统自动在内部新建一个数组，并返回该数组。
13. Comparable 和 Comparator 的区别
    1.  Comparable 接口和 Comparator 接口都是 Java 中用于排序的接口，它们在实现类对象之间比较大小、排序等方面发挥了重要作用：
        1.  Comparable 接口实际上是出自java.lang包 它有一个 compareTo(Object obj)方法用来排序
        2.  Comparator接口实际上是出自 java.util 包它有一个compare(Object obj1, Object obj2)方法用来排序
    2.  我们先从二者的字面含义来理解它，Comparable 翻译为中文是“比较”的意思，而 Comparator 是“比较器”的意思。Comparable 是以 -able 结尾的，表示它自身具备着某种能力，而 Comparator 是以 -or 结尾，表示自身是比较的参与者，这是从字面含义先来理解二者的不同。
    3.  Comparable
        1.  Comparable 接口只有一个方法 compareTo，实现 Comparable 接口并重写 compareTo 方法就可以实现某个类的排序了，它支持 Collections.sort 和 Arrays.sort 的排序。
        2.  compareTo 方法接收的参数 p 是要对比的对象，排序规则是用当前对象和要对比的对象进行比较，然后返回一个 int 类型的值。正序从小到大的排序规则是：使用当前的对象值减去要对比对象的值；而倒序从大到小的排序规则刚好相反：是用对比对象的值减去当前对象的值。
        3.  注意事项：如果自定义对象没有实现 Comparable 接口，那么它是不能使用 Collections.sort 方法进行排序的，编译器会提示如下错误：
    4.  Comparator
        1.  Comparator 排序的方法是 compare
        2.  使用的时候要先创建一个类比较器实例
        3.  Comparator 除了可以通过创建自定义比较器外，还可以通过匿名类的方式，更快速、便捷的完成自定义比较器的功能
    5.  使用场景的区别:
        1.   Comparable 必须要修改原有的类，也就是你要排序那个类，就要在那个中实现 Comparable 接口并重写 compareTo 方法，所以 Comparable 更像是“对内”进行排序的接口
        2.   Comparator 的使用则不相同，Comparator 无需修改原有类。也就是在最极端情况下，即使 Person 类是第三方提供的，我们依然可以通过创建新的自定义比较器 Comparator，来实现对第三方类 Person 的排序功能。也就是说通过 Comparator 接口可以实现和原有类的解耦，在不修改原有类的情况下实现排序功能，所以 Comparator 可以看作是“对外”提供排序的接口。
14.  无序性和不可重复性的含义是什么
     1.   无序性不等于随机性 ，无序性是指存储的数据在底层数组中并非按照数组索引的顺序添加 ，而是根据数据的哈希值决定的。
     2.   不可重复性是指添加的元素按照 equals() 判断时 ，返回 false，需要同时重写 equals() 方法和 hashCode() 方法。
15.  比较 HashSet、LinkedHashSet 和 TreeSet 三者的异同
     1.   HashSet、LinkedHashSet 和 TreeSet 都是 Set 接口的实现类，都能保证元素唯一，并且都不是线程安全的。
     2.  HashSet、LinkedHashSet 和 TreeSet 的主要区别在于底层数据结构不同。HashSet 的底层数据结构是哈希表（基于 HashMap 实现）。LinkedHashSet 的底层数据结构是链表和哈希表，元素的插入和取出顺序满足 FIFO。TreeSet 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
     3.   底层数据结构不同又导致这三者的应用场景不同。HashSet 用于不需要保证元素插入和取出顺序的场景，LinkedHashSet 用于保证元素的插入和取出顺序满足 FIFO 的场景，TreeSet 用于支持对元素自定义排序规则的场景。
16.  Queue 与 Deque 的区别
     1.   Queue 是单端队列，只能从一端插入元素，另一端删除元素，实现上一般遵循 先进先出（FIFO） 规则。
     2.   Queue 扩展了 Collection 的接口，根据 因为容量问题而导致操作失败后处理方式的不同 可以分为两类方法: 一种在操作失败后会抛出异常，另一种则会返回特殊值。
     3.   Deque 是双端队列，在队列的两端均可以插入或删除元素。
     4.   Deque 扩展了 Queue 的接口, 增加了在队首和队尾进行插入和删除的方法，同样根据失败后处理方式的不同分为两类
     5.   Deque 还提供有 push() 和 pop() 等其他方法，可用于模拟栈。
17.  ArrayDeque 与 LinkedList 的区别
     1.   ArrayDeque 和 LinkedList 都实现了 Deque 接口
     2.   ArrayDeque 是基于可变长的数组和双指针来实现，而 LinkedList 则通过链表来实现。
     3.   ArrayDeque 不支持存储 NULL 数据，但 LinkedList 支持。
     4.   ArrayDeque 是在 JDK1.6 才被引入的，而LinkedList 早在 JDK1.2 时就已经存在。
     5.   ArrayDeque 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 LinkedList 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。
     6.   从性能的角度上，选用 ArrayDeque 来实现队列要比 LinkedList 更好。此外，ArrayDeque 也可以用于实现栈。
18.  PriorityQueue
     1.   PriorityQueue 是在 JDK1.5 中被引入的, 其与 Queue 的区别在于元素出队顺序是与优先级相关的，即总是优先级最高的元素先出队。
     2.   PriorityQueue 通过堆元素的上浮和下沉，实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素。
     3.   PriorityQueue 是非线程安全的，且不支持存储 NULL 和 non-comparable 的对象。
     4.   PriorityQueue 默认是小顶堆，但可以接收一个 Comparator 作为构造参数，从而来自定义元素优先级的先后。
19.  什么是 BlockingQueue？
     1.   BlockingQueue （阻塞队列）是一个接口，继承自 Queue。BlockingQueue阻塞的原因是其支持当队列没有元素时一直阻塞，直到有元素；还支持如果队列已满，一直等到队列可以放入新元素时再放入。
     2.   BlockingQueue 常用于生产者-消费者模型中，生产者线程会向队列中添加数据，而消费者线程会从队列中取出数据进行处理。
20.  BlockingQueue 的实现类有哪些？
     1.   ArrayBlockingQueue：使用数组实现的有界阻塞队列。在创建时需要指定容量大小，并支持公平和非公平两种方式的锁访问机制。
     2.   LinkedBlockingQueue：使用单向链表实现的可选有界阻塞队列。在创建时可以指定容量大小，如果不指定则默认为Integer.MAX_VALUE。和ArrayBlockingQueue类似， 它也支持公平和非公平的锁访问机制。
     3.   PriorityBlockingQueue：支持优先级排序的无界阻塞队列。元素必须实现Comparable接口或者在构造函数中传入Comparator对象，并且不能插入 null 元素。
     4.   SynchronousQueue：同步队列，是一种不存储元素的阻塞队列。每个插入操作都必须等待对应的删除操作，反之删除操作也必须等待插入操作。因此，SynchronousQueue通常用于线程之间的直接传递数据。
     5.   DelayQueue：延迟队列，其中的元素只有到了其指定的延迟时间，才能够从队列中出队。
     6.   日常开发中，这些队列使用的其实都不多，了解即可。
21.  ArrayBlockingQueue 和 LinkedBlockingQueue 有什么区别？
     1.   ArrayBlockingQueue 和 LinkedBlockingQueue 是 Java 并发包中常用的两种阻塞队列实现，它们都是线程安全的。不过，不过它们之间也存在下面这些区别：
     2.   底层实现：ArrayBlockingQueue 基于数组实现，而 LinkedBlockingQueue 基于链表实现。
     3.   是否有界：ArrayBlockingQueue 是有界队列，必须在创建时指定容量大小。LinkedBlockingQueue 创建时可以不指定容量大小，默认是Integer.MAX_VALUE，也就是无界的。但也可以指定队列大小，从而成为有界的。
     4.   锁是否分离： ArrayBlockingQueue中的锁是没有分离的，即生产和消费用的是同一个锁；LinkedBlockingQueue中的锁是分离的，即生产用的是putLock，消费是takeLock，这样可以防止生产者和消费者线程之间的锁争夺。
     5.   内存占用：ArrayBlockingQueue 需要提前分配数组内存，而 LinkedBlockingQueue 则是动态分配链表节点内存。这意味着，ArrayBlockingQueue 在创建时就会占用一定的内存空间，且往往申请的内存比实际所用的内存更大，而LinkedBlockingQueue 则是根据元素的增加而逐渐占用内存空间。
22.  HashMap 和 Hashtable 的区别
     1.   线程是否安全： HashMap 是非线程安全的，Hashtable 是线程安全的,因为 Hashtable 内部的方法基本都经过synchronized 修饰。（如果你要保证线程安全的话就使用 ConcurrentHashMap 吧！）；
     2.   效率： 因为线程安全的问题，HashMap 要比 Hashtable 效率高一点。另外，Hashtable 基本被淘汰，不要在代码中使用它；
     3.   对 Null key 和 Null value 的支持： HashMap 可以存储 null 的 key 和 value，但 null 作为键只能有一个，null 作为值可以有多个；Hashtable 不允许有 null 键和 null 值，否则会抛出 NullPointerException。
     4.   初始容量大小和每次扩充容量大小的不同： ① 创建时如果不指定容量初始值，Hashtable 默认的初始大小为 11，之后每次扩充，容量变为原来的 2n+1。HashMap 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。② 创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为 2 的幂次方大小（HashMap 中的tableSizeFor()方法保证，下面给出了源代码）。也就是说 HashMap 总是使用 2 的幂作为哈希表的大小,后面会介绍到为什么是 2 的幂次方。
     5.   底层数据结构： JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）时，将链表转化为红黑树（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树），以减少搜索时间（后文中我会结合源码对这一过程进行分析）。Hashtable 没有这样的机制。
23.  HashMap 和 HashSet 区别
     1.   如果你看过 HashSet 源码的话就应该知道：HashSet 底层就是基于 HashMap 实现的。（HashSet 的源码非常非常少，因为除了 clone()、writeObject()、readObject()是 HashSet 自己不得不实现之外，其他方法都是直接调用 HashMap 中的方法。
     2.   都是线程不安全的
     3.   实现的接口不同
     4.   HashSet不允许重复元素，这意味着您无法在HashSet中存储重复值。HashMap不允许重复键，但它允许重复值。
     5.   HashSet允许具有单个空值。HashMap允许单个null键和任意数量的空值。
24.  HashMap 和 TreeMap 区别
     1.   TreeMap 和HashMap 都继承自AbstractMap ，但是需要注意的是TreeMap它还实现了NavigableMap接口和SortedMap 接口。
     2.   实现 NavigableMap 接口让 TreeMap 有了对集合内元素的搜索的能力。比如lowerEntry(K key) 和 lowerKey(K key): 返回严格小于给定 key 的最大 key 和对应的 Entry。
     3.   实现SortedMap接口让 TreeMap 有了对集合中的元素根据键排序的能力。默认是按 key 的升序排序，不过我们也可以指定排序的比较器。
     4.   综上，相比于HashMap来说 TreeMap 主要多了对集合中的元素根据键排序的能力以及对集合内元素的搜索的能力。
     5.   概括:
          1.   相同:
               1.   都实现Map接口,存储的是键值对
               2.   都是线程不安全的
          2.   不同:
               1.   数据结构:HashMap基于哈希表和链表+红黑树实现,TreeMap基于红黑树实现
               2.   排序:HashMap无序,TreeMap有序
               3.   时间复杂度:HashMap的插入查找复杂度是O(1),TreeMap的插入查找复杂度为O(logn)
               4.   null值:HashMap允许一个null键和多个null值,TreeMap不允许null键,因为他需要使用compareTo方法来对键进行排序,但是它允许null值.
25.  HashSet 如何检查重复?
     1.   当你把对象加入HashSet时，HashSet 会先计算对象的hashcode值来判断对象加入的位置，同时也会与其他加入的对象的 hashcode 值作比较，如果没有相符的 hashcode，HashSet 会假设对象没有重复出现。但是如果发现有相同 hashcode 值的对象，这时会调用equals()方法来检查 hashcode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让加入操作成功。
     2.   HashSet 使用了 HashMap 作为其底层的数据结构，并使用 HashMap 的键来存储元素。这是因为 HashMap 的键是唯一的，因此 HashSet 也就保证了其元素的唯一性。
26.  HashMap解决哈希冲突:
     1.   1.8以前:使用数组+拉链法
     2.   1.8:
          1.   当链表长度大于8,但是数组长度小于64,会先将数组扩容.
          2.   当链表长度大于8,数组长度大于等于64,将链表转化为红黑树
27.  HashMap的成员变量:
     1.   loadFactor 负载因子: 
          1.   loadFactor 负载因子是控制数组存放数据的疏密程度，loadFactor 越趋近于 1，那么 数组中存放的数据(entry)也就越多，也就越密，也就是会让链表的长度增加，loadFactor 越小，也就是趋近于 0，数组中存放的数据(entry)也就越少，也就越稀疏。
          2.   loadFactor 太大导致查找元素效率低，太小导致数组的利用率低，存放的数据会很分散。loadFactor 的默认值为 0.75f 是官方给出的一个比较好的临界值。
          3.   给定的默认容量为 16，负载因子为 0.75。Map 在使用过程中不断的往里面存放数据，当数量超过了 16 * 0.75 = 12 就需要将当前 16 的容量进行扩容，而扩容这个过程涉及到 rehash、复制数据等操作，所以非常消耗性能。
     2.   threshold threshold = capacity * loadFactor，当 Size>threshold的时候，那么就要考虑对数组的扩增了，也就是说，这个的意思就是 衡量数组是否需要扩增的一个标准。
     3.   TREEIFY_THRESHOLD = 8 当桶(bucket)上的结点数大于等于这个值时会转成红黑树
     4.   UNTREEIFY_THRESHOLD = 6 当桶(bucket)上的结点数小于等于这个值时树转链表
     5.   MIN_TREEIFY_CAPACITY = 64;桶中结构转化为红黑树对应的table的最小容量
     6.   Node<k,v>[] table 存储元素的数组，总是2的幂次倍
     7.   int size;存放元素的个数，注意这个不等于数组的长度。
28.  HashMap的put方法?
     1.   调用了一个puVal方法
     2.   判断table成员变量为空,或者长度为0,则需要执行resize(),也就是需要调整大小
     3.   调整完大小后,要根据哈希表的大小(2的整数次幂)和key的哈希计算此节点在哈希表中的位置.
          1.   使用i = (n - 1) & hash 来计算,效率高
     4.   如果哈希表此位置为空,则直接创建一个节点Node加入到这里
     5.   如果哈希表此位置不为空:   
          1.   先判断当前哈希表中的该位置节点,和想要put的节点是否相同,如果相同,则将此节点赋值给e
               1.   if (p.hash == hash && ((k = p.key) == key || (key != null && key.equals(k))))
               2.   先判断hashcode是否相同
               3.   然后用==判断
               4.   然后用equals方法判断
               5.   这种情况不需要插入值,只需要更新值即可
          2.   如果当前哈希表中的该位置节点是一个TreeNode,就在树中插入或更新节点(红黑树的方法)
          3.   如果当前哈希表中的该位置节点不是一个TreeNode,则是一个链表,则在链表中插入或更新
               1.   遍历链表
               2.   如果遍历完链表后,仍然没有找到与要插入节点的key相同的节点,则创建一个新节点,加入到链表中.
                    1.   判断链表长度,如果达到了树化阈值TREEIFY_THRESHOLD,则会进行树形化操作,则会调用treeifyBin方法进行树化
                         1.   treeifyBin中,会进行判断,如果当前的哈希表大小不足MIN_TREEIFY_CAPACITY,则会进行扩容resize()而不是进行树化.
                         2.   如果哈希表达到了MIN_TREEIFY_CAPACITY,则会进行树化
               3.   如果遍历过程中发现有节点的key与要插入节点的key相同,则将其赋值给e
          4.   如果e不为空,则记录当前节点e的value.(e就是通过查找,发现的HashMap中已经存在相同的key的这个节点)
               1.   if (!onlyIfAbsent || oldValue == null) 
               2.   如果onlyIfAbsent为false,则只有在oldValue是null的时候才会将想要插入的元素的value赋值给e
               3.   如果onlyIfAbsent为true,则无论什么情况下,都会将新值赋值给e
     6.   返回值:
          1.   如果新插入的元素的key在原来的HashMap中不存在,则返回null
          2.   反之,则返回原来key的value.
29.  HashMap的resize()方法?
     1.   如果原数组为空，那么就按照 threshold（默认是初始容量的负载因子，初始化时传入）来创建新的 Node 数组。否则，原数组长度翻倍，同时检查长度是否超出最大值（MAXIMUM_CAPACITY = 1 << 30）或者是否小于默认长度。
     2.   对于原数组中的每一个节点，要么保持在原位置，要么移动到原位置再加上旧数组长度的位置。这是一个优化的步骤，通过这种方式可以避免所有键的重新哈希。
          1.   遍历整个旧哈希表中的元素
          2.   当前的旧表元素为null,则新表此位置元素也是null
          3.   如果旧表的当前位置不为bull
               1.   则将旧表的当前位置元素e取出来,并且将此位置设为null
               2.   如果e的下一个元素为空(即该节点不是一个链表或树的一部分),那么就计算它在新数组中的位置
                    1.   e.hash & (newCap - 1)来计算新位置
               3.   如果节点是TreeNode 的实例，那么就使用 TreeNode 的 split 方法将它移到新的位置。这是为了处理 HashMap 中的红黑树部分，这部分内容涉及到了较为复杂的数据结构和算法，超出了这个问题的范围。
               4.   如果节点是一个链表(即节点后面还有其他节点),则会进行链表拆分
                    1.   原数组中一条链表上的所有节点，若将它们加入到扩容后的新数组中，它们最多将会分布在新数组中的两条链表上。
                    2.   (e.hash & oldCap) == 0根据此条件来判断将节点放入原位置的链上,还是放入新位置的链上,新位置就是原位置+oldCap
                    3.   注意这个与运算oldCap是2的n此幂,所以二进制形式是0100(用8举例).
     3.   设置新的阈值： 如果新的阈值不为0，那么阈值设置为新的阈值，否则设置为新的容量乘以负载因子。
30.  HashMap 的长度为什么是 2 的幂次方
     1.   计算哈希表的下标的方法是(n-1)&hashcode,这样计算的必要条件是n是2的整数次幂
     2.   扩容操作只需要左移一位即可.速度快
31.  HashMap 多线程操作导致死循环问题
     1.   1.8之前的版本,在一个桶中有多个元素需要进行扩容的时候,多个线程对链表同时进行操作.插入操作是头插法,头插法会导致链表中的节点指向错误的位置,从而形成一个环形链表,导致查询操作陷入死循环.
     2.   1.8后的版本,使用了链表加红黑树的形式解决哈希碰撞的问题.同时链表的插入操作是尾插法(见putVal),使得插入的节点永远都是放在链表的末尾，避免了链表中的环形结构。但是还是不建议在多线程下使用 HashMap，因为多线程下使用 HashMap 还是会存在数据覆盖的问题。并发环境下，推荐使用 ConcurrentHashMap 。
32.  HashMap 为什么线程不安全？
     1.   JDK1.7 及之前版本，在多线程环境下，HashMap 扩容时会造成死循环和数据丢失的问题。
     2.   JDK 1.8 后，在 HashMap 中，多个键值对可能会被分配到同一个桶（bucket），并以链表或红黑树的形式存储。多个线程对 HashMap 的 put 操作会导致线程不安全，具体来说会有数据覆盖的风险。
33.  ConcurrentHashMap 和 Hashtable 的区别
     1.   它们都是实现了Map接口
     2.   ConcurrentHashMap 和 Hashtable 的区别主要体现在实现线程安全的方式上不同。
     3.   底层数据结构： JDK1.7 的 ConcurrentHashMap 底层采用 分段的数组+链表 实现，JDK1.8 采用的数据结构跟 HashMap1.8 的结构一样，数组+链表/红黑二叉树。Hashtable 和 JDK1.8 之前的 HashMap 的底层数据结构类似都是采用 数组+链表 的形式，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的；