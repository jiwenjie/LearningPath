1. HashMap是map接口的子类，是将键映射到值的对象，其中键和值都是对象，并且不能包含重复键，但可以包含重复值。
     HashMap允许null key和null value，而hashtable不允许。

2. HashMap是Hashtable的轻量级实现（非线程安全的实现），他们都完成了Map接口，由于非线程安全，
     效率上可能高于Hashtable。

3. HashMap把Hashtable的contains方法去掉了，改成containsvalue和containsKey。

4. Hashtable继承自Dictionary类，而HashMap是Java1.2引进的Map interface的一个实现。

5. Hashtable的方法是Synchronize的，而HashMap不是，在多个线程访问Hashtable时，不需要自己为它的方法实现同步，
     而HashMap 就必须为之提供外同步。但是如果使用Java 5或以上的话，可以用ConcurrentHashMap代替Hashtable。

6. Hashtable和HashMap采用的hash/rehash算法都大概一样，所以性能不会有很大的差。