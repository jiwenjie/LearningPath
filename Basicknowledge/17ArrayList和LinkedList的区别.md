###ArrayList和LinkedList的区别

**前言**

[ArrayList](https://link.juejin.im?target=http%3A%2F%2Fwww.liuhaihua.cn%2Farchives%2Ftag%2Farraylist)与[LinkedList](https://link.juejin.im?target=http%3A%2F%2Fwww.liuhaihua.cn%2Farchives%2Ftag%2Flinkedlist)都是List接口的实现类,因此都实现了List的所有未实现的方法,只是实现的方式有所不同,(从中可以看出面向接口的好处, 对于不同的需求就有不同的实现!),而List接口继承了Collection接口,Collection接口又继承了Iterable接口,因此可以看出List同时拥有了Collection与Iterable接口的特性.

****

**ArrayList**

ArrayList是List接口的可变数组的实现。实现了所有可选列表操作，并允许包括 null 在内的所有元素。除了实现 List 接口外，此类还提供一些方法来操作内部用来存储列表的数组的大小。
每个ArrayList实例都有一个容量，该容量是指用来存储列表元素的数组的大小。它总是至少等于列表的大小。随着向ArrayList中不断添加元素，其容量也自动增长。自动增长会带来数据向新数组的重新拷贝，因此，如果可预知数据量的多少，可在构造ArrayList时指定其容量。在添加大量元素前，应用程序也可以使用ensureCapacity操作来增加ArrayList实例的容量，这可以减少递增式再分配的数量。
注意，此实现**不是同步**的。如果多个线程同时访问一个ArrayList实例，而其中至少一个线程从结构上修改了列表，那么它必须保持外部同步。这通常是通过同步那些用来封装列表的对象来实现的。但如果没有这样的对象存在，则该列表需要运用{@link Collections#synchronizedList Collections.synchronizedList}来进行“包装”，该方法最好是在创建列表对象时完成，为了避免对列表进行突发的非同步操作。

```
List list = Collections.synchronizedList(new ArrayList(...));复制代码
```

建议在单线程中才使用ArrayList，而在多线程中可以选择Vector或者CopyOnWriteArrayList。



**扩容**

数组有个明显的特点就是它的容量是**固定不变**的，一旦数组被创建则容量则无法改变。所以在往数组中添加指定元素前，首先要考虑的就是其容量是否饱和。

若接下来的添加操作会时数组中的元素超过其容量，则必须对其进行**扩容**操作。受限于数组容量固定不变的特性，扩容的本质其实就是创建一个**容量更大**的新数组，再将旧数组的元素**复制**到新数组当中去。

这里以 ArrayList 的 添加操作为例，来看下 ArrayList 内部数组扩容的过程。

```
public boolean add(E e) {
	// 关键 -> 添加之前，校验容量
	ensureCapacityInternal(size + 1); 
	
	// 修改 size，并在数组末尾添加指定元素
	elementData[size++] = e;
	return true;
}复制代码
```

可以发现 ArrayList 在进行添加操作前，会检验内部数组容量并**选择性地**进行数组扩容。在 ArrayList 中，通过私有方法 **ensureCapacityInternal** 来进行数组的扩容操作。下面来看具体的实现过程：

- 扩容操作的第一步会去判断当前 ArrayList 内部数组是否为空，为空则将最小容量 minCapacity 设置为 10。

```
// 内部数组的默认容量
private static final int DEFAULT_CAPACITY = 10;

// 空的内部数组
private static final Object[] EMPTY_ELEMENTDATA = {};

// 关键 -> minCapacity = seize+1，即表示执行完添加操作后，数组中的元素个数 
private void ensureCapacityInternal(int minCapacity) {
	// 判断内部数组是否为空
	if (elementData == EMPTY_ELEMENTDATA) {
		// 设置数组最小容量（>=10）
		minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
	}
	ensureExplicitCapacity(minCapacity);
}复制代码
```

- 接着判断添加操作会不会导致内部数组的容量饱和。

```
private void ensureExplicitCapacity(int minCapacity) {
	modCount++;
	
	// 判断结果为 true，则表示接下来的添加操作会导致元素数量超出数组容量
	if (minCapacity - elementData.length > 0){
		// 真正的扩容操作
		grow(minCapacity);
	}
}复制代码
```

- 数组容量不足，则进行扩容操作，关键的地方有两个：**扩容公式**、通过从旧数组复制元素到新数组完成扩容操作。

```
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

private void grow(int minCapacity) {
	
	int oldCapacity = elementData.length;
	
	// 关键-> 容量扩充公式
	int newCapacity = oldCapacity + (oldCapacity >> 1);
	
	// 针对新容量的一系列判断
	if (newCapacity - minCapacity < 0){
		newCapacity = minCapacity;
	}
	if (newCapacity - MAX_ARRAY_SIZE > 0){
		newCapacity = hugeCapacity(minCapacity);
	}
		
	// 关键 -> 复制旧数组元素到新数组中去
	elementData = Arrays.copyOf(elementData, newCapacity);
}

private static int hugeCapacity(int minCapacity) {
	if (minCapacity < 0){
		throw new OutOfMemoryError();
	}
			
	return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;
```



**LinkedList**

1. LinkedList 是一个继承于AbstractSequentialList的双向链表。它也可以被当作堆栈、队列或双端队列进行操作。

2. LinkedList 实现 List 接口，能对它进行队列操作。

3. LinkedList 实现 Deque 接口，即能将LinkedList当作双端队列使用。

4. LinkedList 实现了Cloneable接口，即覆盖了函数clone()，能克隆。

5. LinkedList 实现java.io.Serializable接口，这意味着LinkedList支持序列化，能通过序列化去传输。

6. LinkedList 是非同步的。



**总结**

```
1.ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。
2.对于随机访问get和set，ArrayList觉得优于LinkedList，因为LinkedList要移动指针。
3.对于新增和删除操作add和remove，LinedList比较占优势，因为ArrayList要移动数据。

```



1.  因为 Array 是基于索引 (index) 的数据结构，它使用索引在数组中搜索和读取数据是很快的。Array 获取数据的时间复杂度是 O(1), 但是要删除数据却是开销很大的，因为这需要重排数组中的所有数据。 

2. 相对于 ArrayList , LinkedList 插入是更快的。因为LinkedList不像ArrayList 一样，不需要改变数组的大小，也不需要在数组装满的时候要将所有的数据重新装入一个新的数组，这是   ArrayList   最坏的一种情况，时间复杂度是   O(n)   ，而   LinkedList   中插入或删除的时间复杂度仅为   O(1)  。ArrayList   在插入数据时还需要更新索引（除了插入数组的尾部）。 

3. 类似于插入数据，删除数据时，LinkedList 也优于 ArrayList 。 

4. LinkedList 需要更多的内存，因为 ArrayList 的每个索引的位置是实际的数据，而 LinkedList 中的每个节点中存储的是实际的数据和前后节点的位置( 一个LinkedList实例存储了两个值Node<E> first 和 Node<E> last   分别表示链表的其实节点和尾节点，每个 Node 实例存储了三个值：E item,Node next,Node pre)。 

****

 **什么场景下更适宜使用LinkedList，而不用ArrayList**    

1. 你的应用不会随机访问数据 。因为如果你需要LinkedList中的第n个元素的时候，你需要从第一个元素顺序数到第n个数据，然后读取数据。       

2. 你的应用更多的插入和删除元素，更少的读取数据。因为插入和删除元素不涉及重排数据，所以它要比ArrayList要快。   

3. 以上就是关于ArrayList和LinkedList的差别。你需要一个不同步的基于索引的数据访问时，请尽量使用ArrayList。ArrayList很快，也很容易使用。但是要记得要给定一个合适的初始大小，尽可能的减少更改数组的大小。





