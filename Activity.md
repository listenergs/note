# Activity

## 启动配置

### taskAffinity

任务相似性，在 Manifest 的 activity 标签中通过 android:taskAffinity 属性设置。

### launchMode

启动模式，在 Manifest 的 activity 标签中通过 android:launchMode 属性设置。

1. standard
   默认启动模式，在父 Activity 所在的任务栈中启动（父任务以 singleInstance 模式启动的除外），此模式下忽略 taskAffinity
2. singleTask
   任务栈中只存在一个实例；
   1. 启动流程
      1. 栈中不存在实例：直接正常启动一个实例
      2. 栈顶存在实例：直接复用栈顶的实例，调用 onPause -> onResume
      3. 栈中存在实例：移除实例上边的所有 Activity ，激活栈中的实例，调用 onRestart -> onStart -> onResume
   2. 未设置 taskAffinity
        在父 Activity 所在的任务栈中启动
   3. 与 taskAffinity 配合使用；如果未设置 taskAffinity 则效果等于 standard ；如果 taskAffnity 为空字符串，所有以此模式启动的 Activity 都在单独的任务栈中启动，针对同一 Activity 只会启动一个实例，多次启动只是打开之前的实例；taskAffnity 不为空时， taskAffinity 相同的 Activity 在同一个任务栈中启动，如果
3. singleTop
4. singleInstance
