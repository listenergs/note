# Coroutine （协程）

## 常用方法

### runBlocking

    可以在任意位置开启一个协程，会阻塞当前线程，生命周期小于等于所在线程 。

### launch

    开启一个子协程，生命周期小于等于父协程，返回一个 Job 。

### async

    开启一个子协程，生命周期小于等于父协程，返回一个 Deferred 。

### suspendCoroutine

    将当前执行流挂起，待执行完毕后调用 resume 方法恢复。

### resume

    恢复挂起之前的执行流。

### withContext

    在指定线程中执行。

### withTimeout

    在设置的超时时间内执行一个协程，执行完毕，返回需要的数据，超时则抛出异常 TimeoutCancellationException 。

### withTimeoutOrNull

    功能同 withTimeout ，超时结果不在是抛出异常，而是返回 null 结果

## Continuation

    执行流，两个 suspension point 之间的代码成为泡影一个 Continuation 。

## CoroutineScope

    协程作用域，一般使用方法是继承与 CoroutineScope ，并使用默认的调度器实现，例如： `class BaseActivity :CoroutineScope by CoroutineScope(Dispatchers.Default)` 。

### coroutineScope

    在协程内创建一个作用域，在作用域内的代码块及子协程执行完之前不会结束。

## CoroutineDispatcher

### Dispatchers.Unconfined

    首次挂起前在当前线程执行，恢复后由被调用的挂起函数决定。

### Dispatchers.Default

    使用后台共享线程池，适用于 CPU 密集型任务。

### Dispatchers.IO

    使用按需创建的线程的共享线程池，适用于 IO 密集型任务。

### Dispatchers.Main

    在 UI 线程执行。

## Job

    任务

### cancel

    通过 Job 尝试取消一个协程，也可以说是取消一个协程对应的 Job ，因为通过 launch 启动一个协程时返回的是一个 Job ； cancel 无法直接取消正在执行中的代码，需要通过 isActive 显示的检查取消状态；对于 kotlinx.coroutines 中的挂起函数，都是可以被取消的，并在取消时抛出 CancellationException ，因此也可以在代码中定期调用挂起函数检测取消状态，如 yield 。

### join

    等待协程结束，也可以说是等待 Job 结束，因为通过 launch 启动一个协程时返回的是一个 Job 。

### cancelAndJoin

    等于 Job.cancel+Job.join 。

### 备注

    通常使用 `try{...} finally{...}` 或者 Kotlin 中的 use 函数来处理取消时抛出的异常，在 finally 中作后续处理。

## Deferred

    继承于 Job

### await

    阻塞等待 Job 执行完毕，返回执行结果

## [ThrealLocal 类]([https://www.kotlincn.net/docs/reference/coroutines/coroutine-context-and-dispatchers.html#%E7%BA%BF%E7%A8%8B%E5%B1%80%E9%83%A8%E6%95%B0%E6%8D%AE](https://www.kotlincn.net/docs/reference/coroutines/coroutine-context-and-dispatchers.html#线程局部数据))

    每个线程/协程只能从中取到自己设置的值，并且挂起后该设置失效

## Flow

    与 Sequence 类似，可以用于协程

## Channel

    与 BlockingQueue 类似，用于异步流之间传递数据。

### 创建一个 Channel

1. 使用 Channel() 方法生成，需要手动 close 。
2. 使用 CoroutineScope.produce 方法生成，不需要手动关闭。

### 接收数据

1. 通过 receive 方法依次接收数据。
2. 使用 for(i in channel) 迭代数据。

## [Kotlin Debug](https://github.com/Kotlin/kotlinx.coroutines/tree/master/kotlinx-coroutines-debug)

    testImplementation 'org.jetbrains.kotlinx:kotlinx-coroutines-debug:1.3.3'
