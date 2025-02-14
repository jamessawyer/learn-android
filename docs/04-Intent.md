## 1. Inflater是什么？

android开发中，UI组件存在各种inflater，inflater是什么含义，怎么去理解？

在 Android 开发中，**Inflater** 是一个用于将 XML 布局文件解析为实际的视图（View）对象的工具类。它的核心作用是将定义在 XML 文件中的布局结构转换为可以在 Activity 或 Fragment 中显示的视图层级结构。

### Inflater 的含义

- **Inflater** 的字面意思是“充气器”，在 Android 开发中可以理解为“将布局文件‘充气’成视图对象”的工具。
- 它的主要功能是解析 XML 布局文件，根据文件中的定义创建对应的视图对象，并将这些视图组织成一个视图树。

### 如何理解 Inflater

1. **从 XML 到 View 的转换**：
   - Android 的布局文件通常以 XML 格式编写，定义了界面的结构和样式。Inflater 的作用就是将这些 XML 文件转换为实际的视图对象，以便在应用程序中显示。
   - 例如，一个 XML 文件中定义了一个 `TextView` 和一个 `Button`，Inflater 会根据这些定义创建对应的 `TextView` 和 `Button` 对象。
2. **动态加载布局**：
   - Inflater 允许开发者在运行时动态地加载布局文件。这意味着可以根据不同的条件动态地创建和显示不同的界面。
   - 例如，在一个列表中动态加载每个列表项的布局，或者根据用户的操作动态添加新的视图。
3. **灵活性和复用性**：
   - 使用 Inflater 可以提高代码的复用性。开发者可以定义通用的布局文件，并在多个地方通过 Inflater 加载和使用。
   - 例如，一个通用的列表项布局可以被多个 `RecyclerView` 或 `ListView` 使用。
4. **inflate 方法的参数**：
   - `inflate` 方法通常有三个参数：布局资源 ID、父视图容器和 `attachToRoot` 标志。
   - - **布局资源 ID**：指定要加载的 XML 布局文件。
   - - **父视图容器**：指定加载后的视图将被添加到哪个父视图中。
   - - **attachToRoot**：决定加载后的视图是否立即添加到父视图中。如果为 `true`，加载后的视图会自动添加到父视图中；如果为 `false`，则需要手动调用 `addView` 方法。

### 示例代码

以下是一个简单的示例，展示了如何使用 Inflater 加载布局文件：

java复制

```java
// 获取 LayoutInflater 实例
LayoutInflater inflater = LayoutInflater.from(context);

// 加载 XML 布局文件
View view = inflater.inflate(R.layout.my_layout, null, false);

// 将加载后的视图添加到父视图容器中
container.addView(view);
```

在这个示例中，`R.layout.my_layout` 是要加载的 XML 布局文件的资源 ID，`container` 是父视图容器。

通过理解 Inflater 的工作原理和使用方法，开发者可以更高效地构建和管理 Android 应用的界面。



## 2. Intent是什么？

在 Android 开发中，**Intent** 是一个非常重要的概念，它用于在不同的组件（如 Activity、Service 和 BroadcastReceiver）之间传递消息和请求操作。Intent 是一个对象，封装了组件之间的通信意图，包括要执行的动作、目标组件、数据、额外信息等。

### Intent 的主要用途

1. **启动 Activity**：
   - 用于从一个 Activity 启动另一个 Activity。
   - 可以传递数据和请求结果。
2. **启动 Service**：
   - 用于启动后台服务或与服务进行交互。
3. **发送广播**：
   - 用于向系统或其他应用发送广播消息。
4. **传递数据**：
   - 可以携带额外的数据（通过 `putExtra` 方法）。

### Intent 的类型

Intent 主要分为两种类型：**显式 Intent** 和 **隐式 Intent**。

#### 1. 显式 Intent

显式 Intent 明确指定了目标组件的类名。它主要用于在同一个应用内部启动组件。

**示例：启动另一个 Activity**

kotlin复制

```kotlin
val intent = Intent(this, SecondActivity::class.java)
startActivity(intent)
```

在这个例子中，`Intent` 的构造函数指定了当前的上下文（`this`）和目标 `Activity` 的类名（`SecondActivity::class.java`）。这种方式直接启动了 `SecondActivity`。

#### 2. 隐式 Intent

隐式 Intent 不指定目标组件的类名，而是通过动作（`action`）、类别（`category`）和数据（`data`）等属性来描述要执行的操作。系统会根据这些属性匹配合适的组件来处理 Intent。

**示例：发送短信**

kotlin复制

```kotlin
val intent = Intent(Intent.ACTION_SENDTO, Uri.parse("smsto:1234567890"))
startActivity(intent)
```

在这个例子中，`Intent.ACTION_SENDTO` 是一个标准的动作，表示发送短信，`Uri.parse("smsto:1234567890")` 指定了短信的接收号码。系统会根据这些信息找到合适的短信应用来处理这个 Intent。

### Intent 的主要属性

1. **动作（Action）**：

   - 描述要执行的操作，例如 `Intent.ACTION_VIEW`、`Intent.ACTION_SEND` 等。
   - 自定义动作也可以通过字符串定义，例如 `"com.example.MY_ACTION"`。

2. **类别（Category）**：

   - 进一步描述 Intent 的上下文或用途，例如 `Intent.CATEGORY_DEFAULT`、`Intent.CATEGORY_BROWSABLE` 等。
   - 自定义类别也可以通过字符串定义。

3. **数据（Data）**：

   - 指定 Intent 操作的数据，通常是一个 `Uri` 对象，例如 `Uri.parse("http://www.example.com")`。
   - 可以通过 `<intent-filter>` 中的 `<data>` 标签定义数据的格式。

4. **额外信息（Extras）**：

   - 通过 `putExtra` 方法传递额外的数据，例如键值对。

   kotlin复制

   ```kotlin
   val intent = Intent(this, SecondActivity::class.java)
   intent.putExtra("key", "value")
   startActivity(intent)
   ```

5. **标志（Flags）**：

   - 用于控制 Intent 的行为，例如 `Intent.FLAG_ACTIVITY_NEW_TASK`、`Intent.FLAG_ACTIVITY_CLEAR_TOP` 等。

### Intent 的工作原理

1. **显式 Intent**：
   - 系统直接根据指定的类名启动目标组件。
2. **隐式 Intent**：
   - 系统会根据 Intent 的动作、类别和数据等属性，查找 `AndroidManifest.xml` 中定义的 `<intent-filter>`。
   - 如果找到匹配的组件，则启动该组件；如果没有找到匹配的组件，则会抛出 `ActivityNotFoundException`。

### 示例：定义 `<intent-filter>` 并启动组件

假设你在 `AndroidManifest.xml` 中定义了一个 Activity 的 `<intent-filter>`：

xml复制

```xml
<activity android:name=".SecondActivity">
    <intent-filter>
        <action android:name="com.example.MY_ACTION" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

你可以通过以下代码启动这个 Activity：

kotlin复制

```kotlin
val intent = Intent("com.example.MY_ACTION")
intent.addCategory("android.intent.category.DEFAULT")
startActivity(intent)
```

系统会根据 Intent 的动作和类别，找到匹配的 `SecondActivity` 并启动它。

### 总结

Intent 是 Android 中用于组件之间通信的核心机制。通过 Intent，你可以启动 Activity、Service，发送广播，以及传递数据。理解 Intent 的显式和隐式两种类型以及它们的属性，可以帮助你更好地设计和实现 Android 应用的交互逻辑。



## 3. Intent常用内置ACTION

在 Android 开发中，`Intent` 的 `action` 属性用于描述要执行的操作。系统预定义了许多常用的 `action`，以下是一些常用的内置 `action` 及其作用：

### 常用的内置 `action`

| **Action**                  | **作用**                                                     |
| --------------------------- | ------------------------------------------------------------ |
| `Intent.ACTION_MAIN`        | 用于启动应用的主界面。通常在 `AndroidManifest.xml` 中指定应用的入口点。 |
| `Intent.ACTION_VIEW`        | 用于查看数据，例如打开网页、查看图片或视频等。通常与 `Uri` 结合使用。 |
| `Intent.ACTION_EDIT`        | 用于编辑数据，例如编辑文本文件或数据库中的记录。             |
| `Intent.ACTION_SEND`        | 用于发送数据，例如发送短信、邮件或分享内容到其他应用。       |
| `Intent.ACTION_CALL`        | 用于拨打电话。需要 `CALL_PHONE` 权限。                       |
| `Intent.ACTION_SENDTO`      | 用于发送短信或彩信。需要指定 `smsto:` 或 `mms:` 协议的 `Uri`。 |
| `Intent.ACTION_PICK`        | 用于从数据集中选择一项数据，例如从联系人列表中选择一个联系人。 |
| `Intent.ACTION_GET_CONTENT` | 用于从数据集中获取内容，例如从图库中选择一张图片。           |
| `Intent.ACTION_DIAL`        | 用于启动拨号界面，但不直接拨打电话。                         |
| `Intent.ACTION_SEARCH`      | 用于启动搜索操作，通常与搜索框组件结合使用。                 |

### 示例代码

以下是一些使用这些 `action` 的示例代码：

#### 打开网页

```java
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.example.com"));
startActivity(intent);
```

#### 拨打电话

```java
Intent intent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:1234567890"));
startActivity(intent);
```

#### 发送短信

```java
Intent intent = new Intent(Intent.ACTION_SENDTO, Uri.parse("smsto:1234567890"));
startActivity(intent);
```

#### 分享文本

```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("text/plain");
intent.putExtra(Intent.EXTRA_TEXT, "Hello, this is a shared text!");
startActivity(intent);
```

#### 从图库选择图片

```java
Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
intent.setType("image/*");
startActivity(intent);
```

这些内置 `action` 提供了强大的功能，可以帮助开发者实现各种常见的操作，而无需自己实现复杂的逻辑。



## 4. Intent常用内置CATEGORY

在 Android 开发中，`Intent` 的 `category` 属性用于进一步描述 Intent 的上下文或用途。系统预定义了一些常用的 `category`，以下是一些常见的内置 `category` 及其作用：

### 常用的内置 `category`

| **Category**                           | **作用**                                                     |
| -------------------------------------- | ------------------------------------------------------------ |
| `Intent.CATEGORY_DEFAULT`              | 默认类别。如果一个 Intent 没有明确指定类别，系统会自动添加这个类别。组件必须支持这个类别才能作为默认处理程序。 |
| `Intent.CATEGORY_BROWSABLE`            | 表示该 Intent 可以通过浏览器启动。通常用于处理 HTTP 或 HTTPS 协议的 URI。 |
| `Intent.CATEGORY_TAB`                  | 表示该 Intent 是一个标签页。通常用于多标签页的浏览器或类似场景。 |
| `Intent.CATEGORY_ALTERNATIVE`          | 表示该 Intent 是一个替代操作。通常用于提供多个可选的操作方式。 |
| `Intent.CATEGORY_SELECTED_ALTERNATIVE` | 表示该 Intent 是一个选定的替代操作。通常用于从多个替代操作中选择一个。 |
| `Intent.CATEGORY_LAUNCHER`             | 表示该 Intent 可以从应用启动器启动。通常用于应用的主界面。   |
| `Intent.CATEGORY_INFO`                 | 表示该 Intent 提供关于组件的信息。通常用于显示应用的详细信息。 |
| `Intent.CATEGORY_HOME`                 | 表示该 Intent 是设备的主屏幕。通常用于替换默认的主屏幕应用。 |
| `Intent.CATEGORY_PREFERENCE`           | 表示该 Intent 是一个偏好设置页面。通常用于显示应用的设置界面。 |
| `Intent.CATEGORY_TEST`                 | 表示该 Intent 用于测试目的。通常用于开发和测试阶段。         |
| `Intent.CATEGORY_SAMPLE_CODE`          | 表示该 Intent 是一个示例代码。通常用于展示如何实现特定功能。 |

### 示例代码

以下是一些使用这些 `category` 的示例代码：

#### 启动应用的主界面

```java
Intent intent = new Intent(Intent.ACTION_MAIN);
intent.addCategory(Intent.CATEGORY_LAUNCHER);
startActivity(intent);
```

#### 打开网页（支持浏览器启动）

```java
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.example.com"));
intent.addCategory(Intent.CATEGORY_BROWSABLE);
startActivity(intent);
```

#### 显示应用信息

```java
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.addCategory(Intent.CATEGORY_INFO);
intent.setData(Uri.parse("package:" + getPackageName()));
startActivity(intent);
```

### 总结

这些内置的 `category` 提供了丰富的上下文描述，帮助系统更好地理解 Intent 的用途，并匹配合适的组件来处理 Intent。合理使用这些 `category` 可以使你的应用更加灵活和强大。



## 5. Intent常用内置EXTRA

在 Android 开发中，`Intent` 的 `extras` 是一种用于携带额外数据的方式，这些数据通常以键值对的形式存储在 `Bundle` 中。系统预定义了一些常用的 `extra` 键，用于传递特定类型的数据。以下是一些常见的内置 `extra` 键及其作用：

### 常用的内置 `extra` 键

| **Extra 键**                   | **作用**                                                     |
| ------------------------------ | ------------------------------------------------------------ |
| `Intent.EXTRA_TEXT`            | 用于传递简单的文本内容，通常与 `Intent.ACTION_SEND` 配合使用，用于分享文本。 |
| `Intent.EXTRA_STREAM`          | 用于传递一个 URI，指向要分享的内容（如图片、视频等），通常与 `Intent.ACTION_SEND` 配合使用。 |
| `Intent.EXTRA_SUBJECT`         | 用于传递邮件或消息的主题，通常与 `Intent.ACTION_SEND` 配合使用。 |
| `Intent.EXTRA_EMAIL`           | 用于传递邮件的收件人地址，通常与 `Intent.ACTION_SEND` 配合使用。 |
| `Intent.EXTRA_CC`              | 用于传递邮件的抄送人地址，通常与 `Intent.ACTION_SEND` 配合使用。 |
| `Intent.EXTRA_BCC`             | 用于传递邮件的密送人地址，通常与 `Intent.ACTION_SEND` 配合使用。 |
| `Intent.EXTRA_PHONE_NUMBER`    | 用于传递电话号码，通常与 `Intent.ACTION_CALL` 或 `Intent.ACTION_DIAL` 配合使用。 |
| `Intent.EXTRA_INITIAL_INTENTS` | 用于传递一个 `Intent` 数组，通常用于启动多个 Activity。      |
| `Intent.EXTRA_REFERRER`        | 用于传递来源信息，通常用于统计和分析。                       |
| `Intent.EXTRA_ALLOW_MULTIPLE`  | 用于指示是否允许选择多个项目，通常与 `Intent.ACTION_GET_CONTENT` 配合使用。 |
| `Intent.EXTRA_TITLE`           | 用于传递标题信息，通常用于对话框或选择器。                   |
| `Intent.EXTRA_TEMPLATE`        | 用于传递模板信息，通常用于自定义视图或对话框。               |

### 示例代码

以下是一些使用这些 `extra` 键的示例代码：

#### 分享文本

```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("text/plain");
intent.putExtra(Intent.EXTRA_TEXT, "Hello, this is a shared text!");
startActivity(intent);
```

#### 发送邮件

```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("message/rfc822");
intent.putExtra(Intent.EXTRA_EMAIL, new String[]{"recipient@example.com"});
intent.putExtra(Intent.EXTRA_SUBJECT, "Subject");
intent.putExtra(Intent.EXTRA_TEXT, "Body of the email");
startActivity(intent);
```

#### 拨打电话

```java
Intent intent = new Intent(Intent.ACTION_CALL);
intent.setData(Uri.parse("tel:1234567890"));
intent.putExtra(Intent.EXTRA_PHONE_NUMBER, "1234567890");
startActivity(intent);
```

#### 从图库选择图片

```java
Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
intent.setType("image/*");
intent.putExtra(Intent.EXTRA_ALLOW_MULTIPLE, true);
startActivity(intent);
```

### 总结

这些内置的 `extra` 键提供了标准化的方式，用于在不同组件之间传递特定类型的数据。合理使用这些 `extra` 键可以简化代码，提高代码的可读性和可维护性。同时，这些 `extra` 键也确保了与系统和其他应用的兼容性。



## 6. Intent常用内置FLAG

在 Android 开发中，`Intent` 的 `flag`（标志）用于控制 Intent 的行为，例如如何启动 Activity、如何处理任务栈等。系统预定义了许多常用的 `flag`，以下是一些常见的内置 `flag` 及其作用：

### 常用的内置 `flag`

| **Flag**                                     | **作用**                                                     |
| -------------------------------------------- | ------------------------------------------------------------ |
| `Intent.FLAG_ACTIVITY_NEW_TASK`              | 如果目标 Activity 不在当前任务栈中，则创建一个新的任务栈来启动该 Activity。通常用于从非 Activity 上下文（如 Service 或 BroadcastReceiver）启动 Activity。 |
| `Intent.FLAG_ACTIVITY_SINGLE_TOP`            | 如果目标 Activity 已经位于任务栈的顶部，则不会重新创建新的实例，而是调用 `onNewIntent()` 方法。 |
| `Intent.FLAG_ACTIVITY_CLEAR_TOP`             | 如果目标 Activity 已经存在于任务栈中，则会清除该 Activity 之上的所有 Activity，并将目标 Activity 带到栈顶。 |
| `Intent.FLAG_ACTIVITY_NO_HISTORY`            | 设置该标志后，Activity 不会被保存在任务栈中。用户离开该 Activity 后，系统会立即销毁它。 |
| `Intent.FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS`  | 设置该标志后，Activity 不会出现在最近任务列表中。            |
| `Intent.FLAG_ACTIVITY_MULTIPLE_TASK`         | 允许目标 Activity 创建多个任务栈实例。通常与 `FLAG_ACTIVITY_NEW_TASK` 一起使用。 |
| `Intent.FLAG_ACTIVITY_CLEAR_TASK`            | 清除目标 Activity 所在的任务栈，然后启动目标 Activity。通常与 `FLAG_ACTIVITY_NEW_TASK` 一起使用。 |
| `Intent.FLAG_ACTIVITY_TASK_ON_HOME`          | 将目标 Activity 置于任务栈的根部。                           |
| `Intent.FLAG_ACTIVITY_REORDER_TO_FRONT`      | 如果目标 Activity 已经存在于任务栈中，则将其重新排列到栈顶，而不是创建新的实例。 |
| `Intent.FLAG_ACTIVITY_NO_ANIMATION`          | 启动 Activity 时不显示动画效果。                             |
| `Intent.FLAG_ACTIVITY_PREVIOUS_IS_TOP`       | 表示当前 Activity 的前一个 Activity 仍然位于栈顶。           |
| `Intent.FLAG_ACTIVITY_FORWARD_RESULT`        | 将结果从当前 Activity 转发到目标 Activity。通常用于中间 Activity 的跳转。 |
| `Intent.FLAG_ACTIVITY_RESET_TASK_IF_NEEDED`  | 如果目标 Activity 已经存在于任务栈中，则清除该任务栈并重新启动目标 Activity。 |
| `Intent.FLAG_ACTIVITY_LAUNCHED_FROM_HISTORY` | 表示该 Activity 是从历史记录中启动的。                       |
| `Intent.FLAG_ACTIVITY_NEW_DOCUMENT`          | 从 Android 5.0（API 级别 21）开始支持。用于启动一个新的文档任务。 |
| `Intent.FLAG_ACTIVITY_MULTIPLE_TASK`         | 从 Android 5.0（API 级别 21）开始支持。允许目标 Activity 创建多个任务栈实例。 |
| `Intent.FLAG_ACTIVITY_NEW_DOCUMENT`          | 从 Android 5.0（API 级别 21）开始支持。用于启动一个新的文档任务。 |
| `Intent.FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET` | 设置该标志后，当用户从主屏幕重新启动任务时，系统会清除该任务栈中的所有 Activity。 |

### 示例代码

以下是一些使用这些 `flag` 的示例代码：

#### 启动一个新的任务栈

```java
Intent intent = new Intent(this, SecondActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(intent);
```

#### 清除任务栈并启动目标 Activity

```java
Intent intent = new Intent(this, SecondActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK | Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(intent);
```

#### 如果目标 Activity 已经在栈顶，则不重新创建

```java
Intent intent = new Intent(this, SecondActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
startActivity(intent);
```

#### 清除目标 Activity 之上的所有 Activity

```java
Intent intent = new Intent(this, SecondActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
startActivity(intent);
```

#### 启动 Activity 时不显示动画

```java
Intent intent = new Intent(this, SecondActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_NO_ANIMATION);
startActivity(intent);
```

### 总结

这些内置的 `flag` 提供了强大的控制能力，用于管理 Activity 的启动方式和任务栈的行为。合理使用这些 `flag` 可以帮助你实现复杂的导航逻辑，优化用户体验。



## 7. kotlin中Intent(this, SecondActivity::class.java)是什么意思

在 Kotlin 中，`SecondActivity::class.java` 的含义是获取 `SecondActivity` 类的 Java 类对象（`Class<SecondActivity>`）。这是 Kotlin 中对 Java 的反射机制的一种调用方式。

### 为什么需要 `SecondActivity::class.java`？

在 Android 开发中，`Intent` 用于在不同的组件（如 Activity、Service 等）之间进行通信。当你创建一个 `Intent` 来启动一个新的 `Activity` 时，需要指定目标 `Activity` 的类对象。在 Java 中，可以直接使用 `.class` 来获取类对象，但在 Kotlin 中，语法略有不同。

#### Java 示例：

```java
Intent intent = new Intent(this, SecondActivity.class);
startActivity(intent);
```

#### Kotlin 示例：

```kotlin
val intent = Intent(this, SecondActivity::class.java)
startActivity(intent)
```

### 解释 `SecondActivity::class.java`

1. **`SecondActivity::class`**：
   - `SecondActivity::class` 是 Kotlin 的语法，用于获取 `SecondActivity` 的 `KClass` 对象。`KClass` 是 Kotlin 的反射机制中表示类的类型。
   - 在 Kotlin 中，`::class` 是一个属性，用于获取类的元信息。
2. **`.java`**：
   - `.java` 是将 `KClass` 对象转换为 Java 的 `Class` 对象。这是因为 Android 的 `Intent` 构造函数需要的是 Java 的 `Class` 类型，而不是 Kotlin 的 `KClass` 类型。
   - 这种转换是必要的，因为 Kotlin 的反射机制与 Java 的反射机制在底层实现上有所不同。

### Kotlin 的反射与 Java 的反射

- **Java 的反射**：
  - 在 Java 中，`SecondActivity.class` 直接返回一个 `Class<SecondActivity>` 对象。
  - 这是一个 Java 的 `Class` 类型，用于表示运行时的类信息。
- **Kotlin 的反射**：
  - 在 Kotlin 中，`SecondActivity::class` 返回一个 `KClass<SecondActivity>` 对象。
  - `KClass` 是 Kotlin 的反射机制中的一个类，用于表示类的元信息。
  - 通过 `.java` 属性，可以将 `KClass` 转换为 Java 的 `Class` 对象，以便与 Java 的 API 兼容。

### 总结

在 Kotlin 中，`SecondActivity::class.java` 的含义是：

1. 使用 `SecondActivity::class` 获取 `SecondActivity` 的 `KClass` 对象。
2. 使用 `.java` 将 `KClass` 对象转换为 Java 的 `Class` 对象。
3. 将这个 `Class` 对象传递给 `Intent` 的构造函数，以便启动目标 `Activity`。

这种语法是 Kotlin 与 Java 互操作的一种方式，确保 Kotlin 代码能够无缝地调用 Java 的 API。



## 8. AndroidManifest.xml中的intent-filter

在 `AndroidManifest.xml` 文件中，`<intent-filter>` 标签用于定义一个组件（如 `Activity`、`Service` 或 `BroadcastReceiver`）可以响应的 Intent 类型。通过 `<intent-filter>`，你可以指定组件能够处理的 Intent 的具体特征，例如动作（`action`）、类别（`category`）和数据（`data`）。

以下是你提供的 `<intent-filter>` 配置的详细解释：

### 配置内容

```xml
<intent-filter>
    <action android:name="com.example.ch01hello.ACTION_START2" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="com.example.ch01hello.MY_CATEGORY" />
</intent-filter>
```

### 各部分的含义

1. **`<action>` 标签**
   - `<action android:name="com.example.ch01hello.ACTION_START2" />`
   - **含义**：指定该组件可以响应的动作（Action）。动作是 Intent 的一个属性，用于描述 Intent 的行为。
   - **`android:name`**：动作的名称，必须是唯一的字符串。在这个例子中，动作名称是 `com.example.ch01hello.ACTION_START2`。
   - **作用**：当一个 Intent 的 `action` 属性与这个值匹配时，该组件（如 `Activity`）会被视为可以处理这个 Intent。
2. **`<category>` 标签**
   - `<category android:name="android.intent.category.DEFAULT" />`
   - **含义**：指定该组件可以响应的类别（Category）。类别是 Intent 的一个属性，用于进一步描述 Intent 的上下文或用途。
   - **`android:name`**：类别的名称，必须是唯一的字符串。在这个例子中，有两个类别：
     - `android.intent.category.DEFAULT`：这是 Android 中的一个标准类别，表示该组件可以作为默认的处理程序。如果一个 Intent 没有明确指定类别，系统会自动添加 `DEFAULT` 类别。
     - `com.example.ch01hello.MY_CATEGORY`：这是一个自定义类别，名称为 `com.example.ch01hello.MY_CATEGORY`。
   - **作用**：当一个 Intent 的 `category` 属性包含这些值时，该组件会被视为可以处理这个 Intent。

### 总体含义

这个 `<intent-filter>` 配置表示该组件（如 `Activity`）可以处理以下类型的 Intent：

- **动作**：`com.example.ch01hello.ACTION_START2`
- **类别**：`android.intent.category.DEFAULT` 和 `com.example.ch01hello.MY_CATEGORY`

只有当一个 Intent 的 `action` 和 `category` 属性同时满足以下条件时，该组件才会被启动：

- `action` 属性必须是 `com.example.ch01hello.ACTION_START2`。
- `category` 属性必须包含 `android.intent.category.DEFAULT` 和 `com.example.ch01hello.MY_CATEGORY`。

### 示例代码

以下是一个启动该 `Activity` 的示例代码：

kotlin复制

```kotlin
val intent = Intent("com.example.ch01hello.ACTION_START2").apply {
    // DEFAULT 也可以省略，只写下面一个category
    addCategory("android.intent.category.DEFAULT")
    addCategory("com.example.ch01hello.MY_CATEGORY")
}
startActivity(intent)
```

在这个示例中：

- `Intent` 的 `action` 设置为 `"com.example.ch01hello.ACTION_START2"`。
- `addCategory` 方法用于添加两个类别：`"android.intent.category.DEFAULT"` 和 `"com.example.ch01hello.MY_CATEGORY"`。

只有当 Intent 的 `action` 和 `category` 属性与 `<intent-filter>` 中定义的完全匹配时，该 `Activity` 才会被启动。



### `<data>` 标签的属性

在 Android 的 `<intent-filter>` 中，`<data>` 标签用于指定组件（如 Activity、Service 或 BroadcastReceiver）可以处理的数据类型和 URI 格式。通过 `<data>` 标签，你可以定义组件能够响应的数据类型和 URI 结构。

`<data>` 标签可以包含以下属性，用于定义 URI 的各个部分和数据类型：

- **`android:scheme`**：指定 URI 的协议部分，例如 `http`、`https`、`content` 等。这是定义 URI 的最基本部分，如果没有指定 `scheme`，其他 URI 属性将被忽略。
- **`android:host`**：指定 URI 的主机部分，例如 `www.example.com`。如果未指定 `host`，则 `port` 和路径相关的属性将被忽略。
- **`android:port`**：指定 URI 的端口号。只有在指定了 `host` 的情况下，`port` 才有意义。
- **`android:path`**：指定 URI 的路径部分，例如 `/path/to/resource`。
- **`android:pathPrefix`**：指定路径的前缀，用于匹配以该前缀开头的路径。
- **`android:pathSuffix`**：指定路径的后缀，用于匹配以该后缀结尾的路径。
- **`android:pathPattern`**：指定路径的模式，使用通配符匹配路径。
- **`android:pathAdvancedPattern`**：从 Android 12（API 级别 31）开始支持，允许更复杂的路径匹配。
- **`android:mimeType`**：指定数据的 MIME 类型，例如 `image/jpeg` 或 `text/plain`。可以使用通配符 `*` 来匹配任何子类型。

### 示例

以下是一个包含 `<data>` 标签的 `<intent-filter>` 示例：

xml复制

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="http" android:host="www.example.com" android:pathPrefix="/articles" />
</intent-filter>
```

这个配置表示该组件可以处理以下类型的 Intent：

- 动作是 `android.intent.action.VIEW`。
- 类别是 `android.intent.category.DEFAULT`。
- 数据的 URI 必须以 `http://www.example.com/articles` 开头。

### 注意事项

- 如果 `<intent-filter>` 中没有 `<data>` 标签，则组件可以处理任何数据类型。
- 如果指定了 `<data>` 标签，则 Intent 的数据必须匹配 `<data>` 标签中定义的 URI 和 MIME 类型。
- URI 的匹配是部分匹配，即 Intent 的 URI 只需要与 `<data>` 标签中指定的部分匹配即可。

通过合理配置 `<data>` 标签，你可以精确地定义组件能够处理的数据类型和 URI 结构，从而实现更灵活的 Intent 匹配。



## 9. Intent 数据从一个Activity传递给另一个Activity

假设从 `FirstActivity` 传递数据给 `SecondActivity`

### 1.传递普通数据(字符串，Number, Boolean)

```kotlin
class FirstActivity : AppCompatActivity() {
    private lateinit var binding: FirstLayoutBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = FirstLayoutBinding.inflate(layoutInflater)
        
        enableEdgeToEdge()
        setContentView(binding.root)

        val button1: Button = binding.button1
        button1.setOnClickListener {

            val intent = Intent(this, SecondActivity::class.java).apply {
                // 字符串类型
              putExtra("extra_data", "data from FirstActivity")
            }

            startActivity(intent)
        }

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.firstPage)) {v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}
```

接收数据：

```kotlin
class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.second_layout)

        // 字符串   -> getStringExtra
        // Boolean -> getBooleanExtra
        // Int     -> getIntExtra
        val extraData = intent.getStringExtra("extra_data")
        Log.d("SecondActivity", "extra data is $extraData")

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.secondPage)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}
```



### 2. 传递HashMap



```kotlin
class FirstActivity : AppCompatActivity() {
    private lateinit var binding: FirstLayoutBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = FirstLayoutBinding.inflate(layoutInflater)
        enableEdgeToEdge()
        setContentView(binding.root)
        
        val button1: Button = binding.button1
        button1.setOnClickListener {
            val intent = Intent(this, SecondActivity::class.java).apply {
                putExtra("extra_data", "data from FirstActivity")
                val dataMap = hashMapOf<String, String>(
                    "key1" to "value1",
                    "key2" to "value2"
                )
                putExtra("extra_data", dataMap)
            }

            startActivity(intent)
        }


        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.firstPage)) {v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}
```

接收数据：

```kotlin
class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.second_layout)

        // 使用序列化的方式
        intent.getSerializableExtra("extra_data")?.let {
            for ((key, value) in it as Map<String, String>) {
                Log.d("SecondActivity", "key is $key, value is $value")
            }
        }

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.secondPage)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}
```

另外 `getSerializableExtra(string)` 在 (API level 33 and above）提示已废弃，可以通过AI查看解决办法



## 10. Intent Activity数据返回时传递给上一个Activity

从 点击 `FirstActivity` 按钮跳转到 `SecondActivity`，点击 `SecondActivity` 中的按钮返回数据给 `FirstActivity`：

```kotlin
class FirstActivity : AppCompatActivity() {
    private lateinit var binding: FirstLayoutBinding
    private lateinit var startForResult: ActivityResultLauncher<Intent>

    companion object {
        const val REQUEST_CODE_SECOND_ACTIVITY = 123
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = FirstLayoutBinding.inflate(layoutInflater)
        enableEdgeToEdge()

        setContentView(binding.root)

        startForResult = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) {
            // 使用回调的方式获取返回的数据
            if (it.resultCode == Activity.RESULT_OK) {
                val intent = it.data
                val resultData = intent?.getStringExtra("data_return")
                Toast.makeText(this, resultData, Toast.LENGTH_LONG).show()
            }
        }

//        val button1:Button = findViewById(R.id.button1)
        val button1: Button = binding.button1
        button1.setOnClickListener {

            // 跳转到SecondActivity
            val intent = Intent(this, SecondActivity::class.java)
            startForResult.launch(intent)

            // startActivityForResult的方式已过时
//            startActivityForResult(intent, REQUEST_CODE_SECOND_ACTIVITY)
        }


        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.firstPage)) {v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}
```

返回时传递数据：

```kotlin
class SecondActivity : AppCompatActivity() {
    private lateinit var binding: SecondLayoutBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = SecondLayoutBinding.inflate(layoutInflater)
        enableEdgeToEdge()
        setContentView(binding.secondPage)

        binding.button2.setOnClickListener {
            Log.d("FirstActivity return", "button2 clicked")
            val intent = Intent().apply {
                Log.d("FirstActivity return", "button2 intent")
                putExtra("data_return", "Hello FirstActivity From SecondActivity")
            }
            // 将数据传递给上一个Activity
            setResult(RESULT_OK, intent)
            finish()
        }

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.secondPage)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }
    }
}
```

需要注意 `startActivityForResult` api已过时，配合 `onActivityResult` 在API35中无法正常获取返回的数据



2025年2月14日 09:30:09