## Final Finally Finalize 区别

这是一个经典的 Java 面试题，但是，其实他们这三并没有什么卵联系，只是因为他们长得比较像，才经常被拿出来比较_(:з)∠) ，那么，我们今天也来谈谈，他们这三个分别是什么东东

### Final 

``Final`` 相信大家经常都会用到。在翻译上，``Final``  表示 **最终 **的意思，那在 Java 世界中，``Final`` 也表示最终，不可被继承或者修改的意思。一般我们用到``Final`` 的有三个场景

- 修饰类。当`` Final`` 遇到``Class`` ，这个``Class`` 就变得无法被继承，无法被拓展的类。

- 修饰方法。当``Final`` 修饰了方法，那这个方法将无法被重写 (Override) 

- 修饰变量。当 ``Final`` 修饰了变量，那这个变量将无法被重新赋值及修改，值得注意的是，只是这个变量无法被赋值及修改，而其行为并不受到影响，例如以下代码：

  ```java
  final ArrayList<String>list = new ArrayList<>();
  list.add("aa");
  list.add("bb");
  list =new ArrayList<>(); //报错，list无法被重新赋值
  ```

  上面这段代码的`` list `` 被``Final`` 修饰了，不能被重新改变，重新赋值，但是并不影响他的行为—``add``  。

### Finally

``Finally``  是一种保证重点代码一定被执行的机制，我们可以通过使用 `` try-catch-finally`` ,`` try-finally`` 来进行资源的回收和释放，如文件流的关闭，数据库连接的关闭等。

而使用了``try-with-resources`` 语句，对于资源类操作，我们可以不需要在``finally`` 进行资源的关闭和回收等操作。对于 ``finally`` 我们只需要掌握怎么用就足够了，并没有太多的知识点。

``Finally`` 只有在一种情况下不会被执行，如下代码：

```java
try {
    //do something
    System.exit(1);
} finally{
    System.out.println("xxxx")
}
```

是的，当在``try`` 中如果停止了虚拟机，则``Finally`` 块并不会被执行。

### Finalize

``Finalize``  是``java.lang.Object`` 类的一个方法，设计之初的目的是为了保证对象在被垃圾收集之前完成特定的资源回收和释放。但是由于``Finalize`` 的执行时间并不确定，无法保证``Finalize`` 执行符合预期，可能会导致性能问题，程序死锁等。因此，并不推荐使用``Finalize`` 来进行资源的回收，甚至在 JDK 9 中，``Finalize``  被标记为 **Deprecated** 。

为什么会造成性能的问题？

> 因为 ``Finalize`` 被设计成在垃圾收集前调用，意味着实现了这个非空方法的对象，是个特殊对象，那么在垃圾回收之前就要 jvm 对其进行额外的处理，这将导致了GC的延迟，本质上拖慢了垃圾回收，消耗了更多性能，甚至，当对象很多，多个对象实现了非空的``Finalize`` 方法，拖慢了垃圾回收，导致大量的对象堆积，甚至导致OOM的产生。

那么，``Finalize``  这么糟糕，有没有什么方法可以解决呢？

- 采用``try - catch -finally `` 可以在`` finally`` 进行资源回收
- 使用完资源后，记得进行显示回收。
- 利用资源池尽量重用
- 采用Cleaner清理机制
- 在Android 中可以通过组件的生命周期进行回收。