# Coroutine

#### 微线程也叫协程（Coroutine）。

子程序，也叫函数，在所有编程语言中都是层级调用，比如A调用B，B在执行的过程中有调用了C，C函数执行完毕返回结果，B执行完返回结果，最后是A执行完毕将结果返回。

所以说子程序调用是通过栈来实现的，一个线程就是执行一个子程序。子程序调用时，总是有一个入口和一个出口（返回），调用顺序是明确的。而协程的调用和子程序不同。协程看上去也是子程序，但是在执行过程中， 子程序内部可以中断，然后转而执行别的子程序，在适当的时候再返回来接着执行。

在一个子程序中中断，去执行其他子程序，不是函数调用，有点类似CPU的中断。

#### 协程（Coroutine）的特点在于 是一个线程执行，那么比多线程好在哪儿呢？最优势就是：

1. 大的协程有极高的执行效率。因为子程序切换不是线程切换，而是由程序自身控制，因此没有线程切换的开销。和多线程相比，线程数量越多，协程的性能优势越明显。
2. 第二大优势是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率远高于多线程。

#### 因为协程是一个线程执行，那么如何利用多核CPU？

* 最简单的方法是**多进程 + 协程**，这既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。

Python对协程的支持是通过generator实现的。在generator中，我们不但可以通过`for`循环来迭代，还可以不断调用`next()`函数获取由`yield`语句返回的下一个值。

但是Python的`yield`不但可以返回一个值，它还可以接收调用者发出的参数。

来看例子：

传统的生产者-消费者模型是一个线程写消息，一个线程取消息，通过锁机制控制队列和等待，但一不小心就可能死锁。

如果改用协程，生产者生产消息后，直接通过`yield`跳转到消费者开始执行，待消费者执行完毕后，切换回生产者继续生产，效率极高：

```python
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        r = '200 OK'

def produce(c):
    c.send(None)
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()

c = consumer()
produce(c)
```

执行结果：

```text
[PRODUCER] Producing 1...
[CONSUMER] Consuming 1...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 2...
[CONSUMER] Consuming 2...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 3...
[CONSUMER] Consuming 3...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 4...
[CONSUMER] Consuming 4...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 5...
[CONSUMER] Consuming 5...
[PRODUCER] Consumer return: 200 OK
```

注意到`consumer`函数是一个`generator`，把一个`consumer`传入`produce`后：

1. 首先调用`c.send(None)`启动生成器；
2. 然后，一旦生产了东西，通过`c.send(n)`切换到`consumer`执行；
3. `consumer`通过`yield`拿到消息，处理，又通过`yield`把结果传回；
4. `produce`拿到`consumer`处理的结果，继续生产下一条消息；
5. `produce`决定不生产了，通过`c.close()`关闭`consumer`，整个过程结束。

整个流程无锁，由一个线程执行，`produce`和`consumer`协作完成任务，所以称为“协程”，而非线程的抢占式多任务。

最后套用Donald Knuth的一句话总结协程的特点：

“子程序就是协程的一种特例。”

























