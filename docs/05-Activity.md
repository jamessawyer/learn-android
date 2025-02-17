## Activity启动模式有哪些？

在 Android 开中发，Activity 的启动模式（Launch Mode）用于控制 Activity 的启动行为以及它在任务栈中的实例化方式。Android 提供了四种主要的启动模式，分别是 `standard`、`singleTop`、`singleTask` 和 `singleInstance`。这些启动模式可以通过在 `AndroidManifest.xml` 文件中设置 `<activity>` 标签的 `android:launchMode` 属性来指定。

以下是四种启动模式的详细解释和它们的行为特点：

### 1. **Standard（标准模式）**

- **特点**：

  - 默认的启动模式。
  - 每次启动 Activity 时，都会创建一个新的实例，并将其压入任务栈。
  - 一个 Activity 可以在任务栈中多次实例化。

- **适用场景**：

  - 适用于大多数不需要特殊控制的 Activity。

- **示例**：

  ```xml
  <activity android:name=".MyActivity" />
  ```

### 2. **SingleTop（单例栈顶模式）**

- **特点**：

  - 如果目标 Activity 已经位于任务栈的顶部，则不会创建新的实例，而是复用当前的实例。
  - 如果目标 Activity 不在栈顶，则会创建新的实例并压入栈顶。
  - 可以在 Activity 中通过 `onNewIntent()` 方法接收新的 Intent。

- **适用场景**：

  - 适用于需要避免重复创建实例的场景，例如播放音乐、查看消息列表等。

- **示例**：

  ```xml
  <activity android:name=".MyActivity"
            android:launchMode="singleTop" />
  ```

### 3. **SingleTask（单任务模式）**

- **特点**：

  - 如果目标 Activity 已经存在于任务栈中，则会清除该 Activity 之上的所有 Activity，并将目标 Activity 带到栈顶。
  - 如果目标 Activity 不存在，则会创建一个新的实例。
  - 目标 Activity 在任务栈中只会有一个实例。

- **适用场景**：

  - 适用于需要全局唯一实例的 Activity，例如应用的主界面或导航主页。

- **示例**：

  ```xml
  <activity android:name=".MyActivity"
            android:launchMode="singleTask" />
  ```

### 4. **SingleInstance（单实例模式）**

- **特点**：

  - 目标 Activity 在整个系统中只会有一个实例，即使从不同的任务栈启动，也会共享同一个实例。
  - 该 Activity 会独占一个任务栈，不会与其他 Activity 共享任务栈。
  - 适用于需要完全独立运行的 Activity，例如系统级的设置页面。

- **适用场景**：

  - 适用于需要全局唯一且独立运行的 Activity。

- **示例**：

  ```xml
  <activity android:name=".MyActivity"
            android:launchMode="singleInstance" />
  ```

### 启动模式的行为总结

| 启动模式         | 是否允许重复实例 | 是否允许多个任务栈 | 是否复用现有实例 |
| ---------------- | ---------------- | ------------------ | ---------------- |
| `standard`       | 是               | 是                 | 否               |
| `singleTop`      | 是               | 是                 | 如果在栈顶则复用 |
| `singleTask`     | 否               | 是                 | 是，清除栈顶     |
| `singleInstance` | 否               | 否                 | 是，独占任务栈   |

### 注意事项

1. **`onNewIntent()` 方法**：

   - 如果使用 `singleTop` 或 `singleTask` 模式，当 Activity 被复用时，可以通过覆盖 `onNewIntent()` 方法来接收新的 Intent。

   ```kotlin
   override fun onNewIntent(intent: Intent) {
       super.onNewIntent(intent)
       setIntent(intent) // 更新当前 Activity 的 Intent
       // 处理新的 Intent
   }
   ```

2. **任务栈管理**：

   - 使用 `singleTask` 或 `singleInstance` 模式时，需要注意任务栈的行为，避免用户在返回时出现意外的导航体验。

3. **兼容性**：

   - 不同的启动模式可能会导致复杂的任务栈行为，建议在设计应用时谨慎选择启动模式，以确保用户体验的一致性。

通过合理选择和使用这些启动模式，可以更好地控制 Activity 的实例化行为，优化应用的导航和用户体验。



## 2. onNewIntent() 的用法

`onNewIntent()` 是 Android 中 `Activity` 的一个回调方法，用于处理在特定启动模式下接收到的新 `Intent`。以下是关于 `onNewIntent()` 的详细用法和注意事项：

### 1. **`onNewIntent()` 的作用**

`onNewIntent()` 方法在以下两种启动模式下会被调用：

- **`singleTop`**：当目标 `Activity` 已经位于任务栈的顶部时，新的 `Intent` 不会创建新的实例，而是调用 `onNewIntent()`。
- **`singleTask`**：当目标 `Activity` 已经存在于任务栈中时，新的 `Intent` 会清除该 `Activity` 之上的所有 `Activity`，并将目标 `Activity` 带到栈顶，同时调用 `onNewIntent()`。

### 2. **如何实现 `onNewIntent()`**

以下是一个完整的实现流程：

#### 步骤 1：配置 `AndroidManifest.xml`

确保目标 `Activity` 的 `launchMode` 设置为 `singleTop` 或 `singleTask`：

```xml
<activity android:name=".MainActivity"
          android:launchMode="singleTop">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```



#### 步骤 2：重写 `onNewIntent()` 方法

在目标 `Activity` 中重写 `onNewIntent()` 方法：

```kotlin
override fun onNewIntent(intent: Intent) {
    super.onNewIntent(intent)
    setIntent(intent) // 更新当前 Activity 的 Intent
    handleIntent(intent) // 处理新的 Intent 数据
}
```



#### 步骤 3：发送新的 `Intent`

从其他地方（如另一个 `Activity` 或按钮点击事件）发送新的 `Intent`：

```kotlin
val newIntent = Intent(this, MainActivity::class.java)
newIntent.action = "com.example.ACTION_NEW_INTENT" // 设置一个唯一的动作
startActivity(newIntent)
```



#### 步骤 4：处理新的 Intent 数据

在 `handleIntent()` 方法中处理新的 `Intent` 数据：

```kotlin
private fun handleIntent(intent: Intent) {
    val action = intent.action
    when (action) {
        "com.example.ACTION_NEW_INTENT" -> {
            // 处理新的 Intent 数据
            Toast.makeText(this, "New Intent received", Toast.LENGTH_SHORT).show()
        }
    }
}
```



### 3. **`onNewIntent()` 的使用场景**

- **更新 UI**：当需要根据新的 `Intent` 数据更新当前 `Activity` 的 UI 时。
- **保留状态**：避免重复初始化 `Activity`，同时结合新的 `Intent` 数据保留用户操作的连续性。
- **处理多窗口模式**：在多窗口模式下，用户可能多次启动同一个 `Activity`，`onNewIntent()` 可以确保 `Activity` 不被重复创建。

### 4. **注意事项**

- **调用 `setIntent()`**：在 `onNewIntent()` 中调用 `setIntent(intent)` 是一个常见做法，用于更新当前 `Activity` 的 `Intent`，以便后续可以通过 `getIntent()` 获取最新的 `Intent`。
- **测试启动模式**：不同的启动模式（`singleTop`、`singleTask`）会影响 `onNewIntent()` 的调用时机，建议在开发过程中充分测试。

通过合理使用 `onNewIntent()`，你可以更好地管理 `Activity` 的生命周期，优化用户体验，尤其是在需要处理多个来源的 `Intent` 时。



## 3. 启动模式除了在AndroidManifest.xml中设置外，通过代码是怎么设置？

除了在 `AndroidManifest.xml` 中设置 Activity 的启动模式外，你还可以通过代码动态地设置启动模式。这通常是通过在 `Intent` 中添加特定的标志（`flag`）来实现的。这些标志会影响 Activity 的启动行为，类似于在 `AndroidManifest.xml` 中设置的 `launchMode`。

### 常用的 Intent 标志

以下是一些常用的 Intent 标志，它们可以用来模拟不同的启动模式：

#### 1. **`FLAG_ACTIVITY_NEW_TASK`**

- **作用**：

  - 如果目标 Activity 不在当前任务栈中，则会创建一个新的任务栈来启动该 Activity。
  - 类似于 `launchMode="singleTask"` 的行为。

- **示例代码**：

  ```kotlin
  val intent = Intent(this, TargetActivity::class.java)
  intent.flags = Intent.FLAG_ACTIVITY_NEW_TASK
  startActivity(intent)
  ```

#### 2. **`FLAG_ACTIVITY_SINGLE_TOP`**

- **作用**：

  - 如果目标 Activity 已经位于任务栈的顶部，则不会创建新的实例，而是调用 `onNewIntent()` 方法。
  - 类似于 `launchMode="singleTop"` 的行为。

- **示例代码**：

  ```kotlin
  val intent = Intent(this, TargetActivity::class.java)
  intent.flags = Intent.FLAG_ACTIVITY_SINGLE_TOP
  startActivity(intent)
  ```

#### 3. **`FLAG_ACTIVITY_CLEAR_TOP`**

- **作用**：

  - 如果目标 Activity 已经存在于任务栈中，则会清除该 Activity 之上的所有 Activity，并将目标 Activity 带到栈顶。
  - 类似于 `launchMode="singleTask"` 的行为。

- **示例代码**：

  ```kotlin
  val intent = Intent(this, TargetActivity::class.java)
  intent.flags = Intent.FLAG_ACTIVITY_CLEAR_TOP
  startActivity(intent)
  ```

#### 4. **`FLAG_ACTIVITY_CLEAR_TASK`**

- **作用**：

  - 清除目标 Activity 所在的任务栈，并启动目标 Activity。
  - 结合 `FLAG_ACTIVITY_NEW_TASK` 使用时，可以完全清空任务栈并启动一个新的实例。

- **示例代码**：

  ```kotlin
  val intent = Intent(this, TargetActivity::class.java)
  intent.flags = Intent.FLAG_ACTIVITY_CLEAR_TASK or Intent.FLAG_ACTIVITY_NEW_TASK
  startActivity(intent)
  ```

#### 5. **`FLAG_ACTIVITY_NEW_DOCUMENT`**

- **作用**：

  - 从 Android 5.0（API 级别 21）开始支持。
  - 用于启动一个新的文档任务，类似于 `launchMode="singleInstance"` 的行为。

- **示例代码**：

  ```kotlin
  val intent = Intent(this, TargetActivity::class.java)
  intent.flags = Intent.FLAG_ACTIVITY_NEW_DOCUMENT
  startActivity(intent)
  ```

### 动态设置启动模式的注意事项

1. **优先级**：
   - 如果在 `AndroidManifest.xml` 中已经设置了 `launchMode`，则代码中设置的 Intent 标志可能会覆盖或与之冲突。因此，建议在代码中明确设置标志，以避免混淆。
2. **组合使用**：
   - 你可以组合使用多个标志来实现复杂的启动行为。例如，`FLAG_ACTIVITY_CLEAR_TOP` 和 `FLAG_ACTIVITY_SINGLE_TOP` 可以一起使用，以确保目标 Activity 始终位于栈顶。
3. **兼容性**：
   - 不同的 Android 版本可能会对 Intent 标志的行为略有差异，建议在目标设备上进行充分测试。

### 示例：动态设置启动模式

以下是一个完整的示例，展示了如何通过代码动态设置启动模式：

```kotlin
val intent = Intent(this, TargetActivity::class.java)

// 设置为 singleTop 模式
intent.flags = Intent.FLAG_ACTIVITY_SINGLE_TOP

// 启动 Activity
startActivity(intent)
```

通过代码设置启动模式，可以让你在运行时灵活地控制 Activity 的行为，而无需在 `AndroidManifest.xml` 中固定设置。这在某些动态场景下非常有用，例如根据用户操作或应用状态动态调整启动行为。