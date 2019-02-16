## Exception和Error

### Throwable

`` Throwable ``  是``Exception``  和`` Error`` 的父类，在 Java 上，只有 ``Throwable`` 及其子类才可以抛出异常。

同样的只有`` Throwable`` 及其子类才可以被``Catch`` 当作参数进行捕获。

### Exception

根据定义，``Exception``  是在 `` Java`` 平台上可被预料的意外情况，可以且应该被捕获处理的意外情况，然后进行相应的处理，``Exception``  又分为可检查异常和不可检查异常

- 检查型异常

  在编码过程中就要进行显示处理捕获的异常，这通常是在编译器进行检查，如果遇到相应的异常却没有进行及时处理，那么编译器是不会给予通过的

- 非检查型异常

  即所谓的运行时异常，在编译期并不强制要求进行检查，可根据自己的需要进行捕获。只有在运行期间遇到相对应的错误，才会抛出异常，如我们常见的 NPE（NullPointerException），OutOfBoundsException 等。

### Error

``Error ``  是指在正常情况下不太可能出现的情况，当出现 ``Error`` 时，大部分都会引起程序终止工作，且是处于不正常，不可恢复的状态，例如`` OutOfMemoryError`` 等，这类错误我们无法提前感知，即使出现了我们也无法轻易回复状态。

 ### 异常处理

在日常开发中，我们除了要弄明白什么是异常(Exception)，什么是错误(Error)，我们还要知道如何应对，如何处理异常。

在日常开发中，处理异常一般要先捕获，而捕获就是我们常用的`` try - catch - finaly `` 组合套装，在语法方面，除了常用的用法，我们还应该了解在``java`` 提供的便利性，如 ***try-with-resources*** 和 **multiple catch** 

- **try-with-resources** 

  基本语法：参考连接  https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html 

  在try 中通过对实现了AutoCloseable或Closeable接口的类（如BufferedReader），在遇到异常或者正常终止的时候，会自动调用close ，关闭对应的资源，而不需要在finally里进行手动close。

- **multiple catch**
  可捕获多个异常，增大捕获异常的几率

```java
try (BufferedReader br = new BufferedReader(...);
     BufferedWriter writer = new BufferedWriter (...)){ // try-with-resources
    // do samething
}catch (IOException | xxException e){ // multiple catch
    // do samething
}
```

**两个基本原则** ：

- 尽量不要捕获大的异常，而应该捕获特定的异常。如尽量不要捕获``Exception`` 之类的异常，而应该捕获的是``NullPointerException`` 之类的异常，让捕获的异常尽可能有特定性，确保不会错误处理其他异常，甚至，不应该捕获`` Throwable`` 和 ``Error``  ，因为我们并不能很好的在程序里处理诸如`` OutOfMemoryError`` 之类的错误，应该让此类错误暴露出来，让相对应的开发人员尽早处理。
- 不要生吞异常。所谓的生吞异常，指的是当异常出现的时候，并不采用任何持久化的存储将异常保存下来，而是简单的使用`` printStackTrace()`` 输出，甚至没有输出。这可能导致异常并不能暴露给开发人员，反而被程序 “生吞” 了，这可能将导致程序发生异常，而无法进行调试的诡异情况

最后，我们从性能层面分析一下 Java 的异常处理机制，`` try-catch``  会导致程序有额外的性能开销，换个角度说，他往往会影响 JVM 对代码的优化，所以建议只捕获关键的代码段，尽量不要一个大的 ``try`` 捕获一个大的代码块，这将影响性能。并且，Java 每生成一个`` Exception``  都会对当前的堆栈进行快照，生成快照如果比较频繁的话，那这部分的开销将不能忽略了，也别指望通过`` try - catch`` 对代码流程进行管控，在流程把控上还不如`` if else`` 高效，因此，慎重使用`` try catch `` 异常处理机制，确保好钢用在刀刃上。