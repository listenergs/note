# Kotlin Note

## Coroutine （协程）

### 协程相关方法

#### runBlocking

> 可以在任意位置开启一个协程，会阻塞当前线程，生命周期小于等于所在线程

#### launch

> 开启一个子协程，生命周期小于等于父协程，返回一个Job

#### GlobalScope.launch

>利用GlobalScope在任意位置开启一个协程，生命周期小于等于当前应用，返回一个Job

#### async

> 开启一个子协程，生命周期小于等于父协程，返回一个Deferred。

#### GlobalScope.async

> 利用GlobalScope在任意位置开启一个协程，生命周期小于等于当前应用，返回一个Deferred。

#### withTimeout

> 在设置的超时时间内执行一个协程，执行完毕，返回需要的数据，超时则抛出异常 TimeoutCancellationException

#### withTimeoutOrNull

> 功能同withTimeout，超时结果不在是抛出异常，而是返回 null 结果

### 声明一个作用域

####  coroutineScope

> 在协程内创建一个作用域，在作用域内的代码块及子协程执行完之前不会结束

### Job

##### cancel

> 通过Job尝试取消一个协程，也可以说是取消一个协程对应的Job，因为通过launch启动一个协程时返回的是一个Job；cancel无法直接取消正在执行中的代码，需要通过 isActive 显示的检查取消状态；对于 kotlinx.coroutines 中的挂起函数，都是可以被取消的，并在取消时抛出 CancellationException ，因此也可以在代码中定期调用挂起函数检测取消状态，如 yield 。

#### join

> 等待协程结束，也可以说是等待Job结束，因为通过launch启动一个协程时返回的是一个Job

#### cancelAndJoin

> 等于Job.cancel+Job.join

> 备注：
>
> ​	通常使用 `try{...} finally{...}` 或者 Kotlin 中的 use 函数来处理取消时抛出的异常，在 finally 中作后续处理

### Deferred

> 继承与Job

#### await

> 阻塞等待Job执行完毕，返回执行结果

### CoroutineDispatcher

#### Dispatchers.Unconfined

#### Dispatchers.Default

#### Dispatchers.IO

#### Dispatchers.Main

### [ThrealLocal]([https://www.kotlincn.net/docs/reference/coroutines/coroutine-context-and-dispatchers.html#%E7%BA%BF%E7%A8%8B%E5%B1%80%E9%83%A8%E6%95%B0%E6%8D%AE](https://www.kotlincn.net/docs/reference/coroutines/coroutine-context-and-dispatchers.html#线程局部数据))

> 每个线程/协程只能从中取到自己设置的值，并且挂起后该设置失效

### [Kotlin Debug](https://github.com/Kotlin/kotlinx.coroutines/tree/master/kotlinx-coroutines-debug)

> testImplementation 'org.jetbrains.kotlinx:kotlinx-coroutines-debug:1.3.3'