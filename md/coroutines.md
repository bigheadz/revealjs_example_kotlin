
# Coroutines

> Coroutines are *experimental* in Kotlin 1.1.

[ppt]:()
## Gradle setting
```
compile 'org.jetbrains.kotlinx:kotlinx-coroutines-core:0.17'

kotlin {
    experimental {
        coroutines "enable"
    }
}
```

note: 
一些 API 启动长时间运行的操作（例如网络 IO、文件 IO、CPU 或 GPU 密集型任务等），并要求调用者阻塞直到它们完成。协程提供了一种避免阻塞线程并用更廉价、更可控的操作替代线程阻塞的方法：协程挂起。
协程通过将复杂性放入库来简化异步编程。程序的逻辑可以在协程中顺序地表达，而底层库会为我们解决其异步性。该库可以将用户代码的相关部分包装为回调、订阅相关事件、在不同线程（甚至不同机器！）上调度执行，而代码则保持如同顺序执行一样简单。

[ppt]:()
## Blocking vs Suspending
* blocking 代价较大
* suspending几乎无代价
note: 
基本上，协程计算可以被挂起而无需阻塞线程。线程阻塞的代价通常是昂贵的，尤其在高负载时，因为只有相对少量线程实际可用，因此阻塞其中一个会导致一些重要的任务被延迟。
另一方面，协程挂起几乎是无代价的。不需要上下文切换或者 OS 的任何其他干预。最重要的是，挂起可以在很大程度上由用户库控制：作为库的作者，我们可以决定挂起时发生什么并根据需求优化 / 记日志 / 截获。
另一个区别是，协程不能在随机的指令中挂起，而只能在所谓的挂起点挂起，这会调用特别标记的函数。
  

[ppt]:()
### some examples 

* 运行中的协程并不会保证线程的正常运行，如上面的例子，协程处于挂起状态未执行完成， 主线程结束之后，子协程也直接结束了
* 挂起函数只能由别的挂起函数调用，或者用在协程中，不可以直接在线程中使用

[ppt]:()
### Generators API in `kotlin.coroutines`
  
Kotlin中唯一应用级别的api
- [`buildSequence()`](/api/latest/jvm/stdlib/kotlin.coroutines.experimental/build-sequence.html)
- [`buildIterator()`](/api/latest/jvm/stdlib/kotlin.coroutines.experimental/build-iterator.html)


[ppt]:()

``` kotlin
    import kotlin.coroutines.experimental.*
    val lazySeq = buildSequence {
        print("START ")
        for (i in 1..5) {
            yield(i)
            print("STEP ")
        }
        print("END")
    }

    // Print the first three elements of the sequence
    lazySeq.take(3).forEach { print("$it ") }
```