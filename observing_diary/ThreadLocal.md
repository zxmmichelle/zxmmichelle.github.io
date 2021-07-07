## 2021年7月7日
> 看RabbitTemplate源码时，发现Channel被放进了ThreadLocal里。刚好最近又看了一下相关的题目，各路up主大神绕得我头都晕，决定自己总结一篇我自己能看懂的。  
> 
> 首先，Thread源码中有一个声明
> ```java
> ThreadLocal.ThreadLocalMap threadLocals = null;
> ```
> 其次，ThreadLocal的使用步骤大致如下，在某class中声明ThreadLocal，接着在方法中调用get()方法获取值
> ```java
> private ThreadLocal<?> locals = new ThreadLocal<>();
> ...
> locals.set(?);
> ...
> locals.get();
> ```
> 这里的用法就很容易误会ThreadLocal是存在某class中（对于我来说）。实际上，在ThreadLocal中的set()方法里，是这样处理的
> ```java
> public void set(T value) {
>    Thread t = Thread.currentThread();
>    ThreadLocalMap map = getMap(t);
>    if (map != null) {
>        map.set(this, value);
>    } else {
>        createMap(t, value);
>    }
>  }
> ```
> set()方法中，先获取当前线程，再从当前线程中取ThreadLocalMap，结合一开始的Thread源码，可以得出ThreadLocal是作为Key存放在Thread的ThreadLocalMap中的。   
> 插一句题外话，JVM中的栈帧是不保存自定义对象实例的，所以在JVM层面上，ThreadLocalMap实际是在Heap中。  
>
> 搞清楚这套逻辑以后，接着看get()方法
> ```java
> public T get(){
>   Thread t = Thread.currentThread();
>   ThreadLocalMap map = getMap(t);
>   if (map != null) {
>     ThreadLocalMap.Entry e = map.getEntry(this);
>     if (e != null) {
>       @SuppressWarnings("unchecked")
>       T result = (T)e.value;
>       return result;
>     }
>   }
>   return setInitialValue();
> }
> ```
> get()方法里，获取不到map的时候，就会初始化一个该ThreadLocal的Entry（在setInitialValue里，懒得贴出来了），并设置value为null。如果有map的时候，会尝试去getEntry()。  
> 这里有个新疑问，为什么要专门写个ThreadLocalMap和Entry，原来的HashMap不香吗？  
> 源码中针对ThreadLocalMap有一段注释，如下
> ```java
>  /**
>   * ThreadLocalMap is a customized hash map suitable only for
>   * maintaining thread local values. No operations are exported
>   * outside of the ThreadLocal class. The class is package private to
>   * allow declaration of fields in class Thread.  To help deal with
>   * very large and long-lived usages, the hash table entries use
>   * WeakReferences for keys. However, since reference queues are not
>   * used, stale entries are guaranteed to be removed only when
>   * the table starts running out of space.
>   */
> ```
> 捡几句重点的翻译一下，为了适配这种生命周期与线程生命周期等长的局部变量，所以特意写了这个ThreadLocalMap。为了处理庞大且声明周期长的变量，这个Entry类特地对key做了弱引用声明。同时建议用完就remove一下key，避免ThreadLocalMap耗费过多的空间资源。
>
> 最后，回到开头的RabbitTemplate，让每个调用RabbitTemplate的线程，都拥有一个自己的Channel，真妙啊！不过如果这个Channel断开连接了怎么办？
