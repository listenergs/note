# [基础篇](https://juejin.im/post/5c85cead5188257c6703af47)

## Activity

1. 生命周期
2. 四种启动模式
3. [Activity 跟 Window、View 之间的关系](https://blog.csdn.net/zane402075316/article/details/69822438)
   1. 基本
      1. Activity在创建时会调用 attach() 方法初始化一个PhoneWindow(继承于Window)，每一个Activity都包含了唯一一个PhoneWindow
      2. Activity通过setContentView实际上是调用的 getWindow().setContentView将View设置到PhoneWindow上，而PhoneWindow内部是通过 WindowManager 的addView、removeView、updateViewLayout这三个方法来管理View，WindowManager本质是接口，最终由WindowManagerImpl实现
   2. 延伸
      1. WindowManager为每个Window创建Surface对象，然后应用就可以通过这个Surface来绘制任何它想要绘制的东西。而对于WindowManager来说，这只不过是一块矩形区域而已
         1. Surface其实就是一个持有像素点矩阵的对象，这个像素点矩阵是组成显示在屏幕的图像的一部分。我们看到显示的每个Window（包括对话框、全屏的Activity、状态栏等）都有他自己绘制的Surface。而最终的显示可能存在Window之间遮挡的问题，此时就是通过SurfaceFlinger对象渲染最终的显示，使他们以正确的Z-order显示出来。一般Surface拥有一个或多个缓存（一般2个），通过双缓存来刷新，这样就可以一边绘制一边加新缓存。
      2. View是Window里面用于交互的UI元素。Window只attach一个View Tree（组合模式），当Window需要重绘（如，当View调用invalidate）时，最终转为Window的Surface，Surface被锁住（locked）并返回Canvas对象，此时View拿到Canvas对象来绘制自己。当所有View绘制完成后，Surface解锁（unlock），并且post到绘制缓存用于绘制，通过Surface Flinger来组织各个Window，显示最终的整个屏幕
4. 横竖屏切换的Activity生命周期变化
   1. 不设置 Activity的android:configChanges 时，切屏会销毁当前Activity，然后重新加载调用各个生命周期，切横屏时会执行一次，切竖屏时会执行两次；onPause() → onStop() → onDestory() → onCreate() → onStart() → onResume()
   2. 设置 Activity 的 android:configChanges="orientation" (待验证：在 Android5.1 即 API 23级别下，切屏还是会重新调用各个生命周期，切横、竖屏时只会执行一次在Android9 即API 28级别下，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法)；[官网说明](https://developer.android.com/guide/topics/manifest/activity-element)：如果您的应用面向Android 3.2即API 级别 13或更高级别（按照 minSdkVersion 和 targetSdkVersion 属性所声明的级别），则还应声明 "screenSize" 配置，因为当设备在横向与纵向之间切换时，该配置也会发生变化。即便是在 Android 3.2 或更高版本的设备上运行，此配置变更也不会重新启动 Activity
5. Service
6. Broadcast Receiver
7. [ContentProvider]([Android：关于ContentProvider的知识都在这里了！](https://blog.csdn.net/carson_ho/article/details/76101093))
   1. ContentProvider 了解多少?
      1. ContentProvider作为四大组件之一，其主要负责存储和共享数据。与文件存储、SharedPreferences 存储、SQLite数据库存储这几种数据存储方法不同的是，后者保存下的数据只能被该应用程序使用，而前者可以让不同应用程序之间进行数据共享，它还可以选择只对哪一部分数据进行共享，从而保证程序中的隐私数据不会有泄漏风险。
   2. ContentProvider 的权限管理？
      1. 读写分离
      2. 权限控制-精确到表级
      3. URL控制
   3. ContentProvider 、 ContentResolver 、 ContentObserver
      1. ContentProvider ：管理数据，提供数据的增删改查操作，数据源可以是数据库、文件、 XML 、网络等， ContentProvider 为这些数据的访问提供了统一的接口，可以用来做进程间数据共享。
      2. ContentResolver ： ContentResolver 可以为不同 URI 操作不同的 ContentProvider 中的数据，外部进程可以通过 ContentResolver 与 ContentProvider 进行交互。
      3. ContentObserver ：观察 ContentProvider 中的数据变化，并将变化通知给外界。
8. 数据存储
   1. Android 数据持久存储方式
      1. SharedPreferences 存储：一种轻型的数据存储方式，本质是基于 XML 文件存储的 key-value 键值对数据，通常用来存储一些简单的配置信息（如应用程序的各种配置信息）；
      2. SQLite 数据库存储：一种轻量级嵌入式数据库引擎，它的运算速度非常快，占用资源很少，常用来存储大量复杂的关系数据；
      3. ContentProvider ：四大组件之一，用于数据的存储和共享，不仅可以让不同应用程序之间进行数据共享，还可以选择只对哪一部分数据进行共享，可保证程序中的隐私数据不会有泄漏风险；
      4. File 文件存储：写入和读取文件的方法和 Java中实现I/O的程序一样；
      5. 网络存储：主要在远程的服务器中存储相关数据，用户操作的相关数据可以同步到服务器上；
   2. [SharedPreferences 的应用场景及注意事项](https://blog.csdn.net/geekerhw/article/details/79713068)
      1. SharedPreferences 是一种轻型的数据存储方式，本质是基于XML文件存储的 key-value 键值对数据，通常用来存储一些简单的配置信息，如 int 、 String 、 boolean 、 float 和 long ；
      2. 注意事项：
         1. 勿存储大型复杂数据，这会引起内存 GC 、阻塞主线程使页面卡顿产生 ANR
         2. 勿在多进程模式下，操作 Sp
         3. 不要多次 edit 和 apply ，尽量批量修改一次提交
         4. 建议 apply ，少用 commit
      3. SharedPrefrences 的 apply 和 commit 有什么区别
      4. 了解 SQLite 中的事务操作吗？是如何做的
         1. SQLite 在做 CRDU 操作时都默认开启了事务，然后把 SQL 语句翻译成对应的 SQLiteStatement 并调用其相应的 CRUD 方法，此时整个操作还是在 rollback journal 这个临时文件上进行，只有操作顺利完成才会更新 db 数据库，否则会被回滚；
      5. 使用 SQLite 做批量操作有什么好的方法吗
         1. 使用 SQLiteDatabase 的 beginTransaction 方法开启一个事务，将批量操作 SQL 语句转化为 SQLiteStatement 并进行批量操作，结束后 endTransaction()
      6. 如何删除 SQLite 中表的个别字段
         1. SQLite数据库只允许增加字段而不允许修改和删除表字段，只能创建新表保留原有字段，删除原表
      7. 使用SQLite时会有哪些优化操作
         1. 
