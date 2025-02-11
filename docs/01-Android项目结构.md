## 1. 列举一下android项目开发中，Gradle Scripts下各个文件的作用

1. build.gradle (Project: AmoyApp) 
2. build.gradle(Module :app) 
3. gradle.properties(Project Properties)
4. gradle.properties(GlobalProperties)
5. gradle-wrapper.properties(Gradle version) 
6. local.properties(SDK Location)

在Android项目开发中，`Gradle Scripts`目录下的各个文件都有特定的作用，以下是它们的详细说明：

### 1. **`build.gradle (Project: AmoyApp)`**

- **作用**：

  - 这是项目的根`build.gradle`文件，定义了整个项目的配置信息，适用于所有模块。
  - 主要用于配置项目的依赖管理、插件应用、构建脚本的通用设置等。

- **常见内容**：

  - 定义项目使用的插件（如`com.android.application`、`com.android.library`等）。
  - 配置项目的依赖（如`classpath`依赖）。
  - 定义全局的变量或配置，供所有模块共享。

- **示例**：

  ```groovy
  // 项目根目录的 build.gradle 文件
  buildscript {
      repositories {
          google()
          mavenCentral()
      }
      dependencies {
          classpath 'com.android.tools.build:gradle:7.0.0'
          classpath 'com.google.gms:google-services:4.3.10'
      }
  }
  
  allprojects {
      repositories {
          google()
          mavenCentral()
      }
  }
  ```

### 2. **`build.gradle (Module: app)`**

- **作用**：

  - 这是模块级别的`build.gradle`文件，定义了特定模块（如`app`模块）的构建配置。
  - 主要用于配置模块的构建类型、产品风味(`productFlavors`)、依赖管理、签名配置等。

- **常见内容**：

  - 配置模块的`applicationId`、`minSdkVersion`、`targetSdkVersion`等。
  - 定义构建类型（如`release`、`debug`）和产品风味。
  - 管理模块的依赖（如`implementation`、`testImplementation`等）。

- **示例**：

  ```groovy
  // app 模块的 build.gradle 文件
  plugins {
      id 'com.android.application'
      id 'kotlin-android'
  }
  
  android {
      compileSdkVersion 30
      defaultConfig {
          applicationId "com.example.amoyapp"
          minSdkVersion 21
          targetSdkVersion 30
          versionCode 1
          versionName "1.0"
      }
      buildTypes {
          release {
              minifyEnabled true
              proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
          }
      }
  }
  
  dependencies {
      implementation 'androidx.core:core-ktx:1.6.0'
      implementation 'androidx.appcompat:appcompat:1.3.1'
      testImplementation 'junit:junit:4.13.2'
  }
  ```

### 3. **`gradle.properties (Project Properties)`**

- **作用**：

  - 项目级别的`gradle.properties`文件，用于定义项目的全局属性和配置。
  - 可以在这里设置一些全局变量，如编译参数、依赖版本等。

- **常见内容**：

  - 设置`org.gradle.jvmargs`来配置Gradle的JVM参数。
  - 定义全局的依赖版本号，方便统一管理。

- **示例**：

  properties复制

  ```properties
  # 项目级别的 gradle.properties 文件
  org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
  android.useAndroidX=true
  android.enableJetifier=true
  ```

### 4. **`gradle.properties (Global Properties)`**

- **作用**：

  - 全局级别的`gradle.properties`文件，通常位于用户的Gradle配置目录下（如`~/.gradle/gradle.properties`）。
  - 用于定义适用于所有项目的全局配置。

- **常见内容**：

  - 设置Gradle的全局参数，如代理设置、缓存配置等。

- **示例**：

  properties复制

  ```properties
  # 全局级别的 gradle.properties 文件
  systemProp.http.proxyHost=127.0.0.1
  systemProp.http.proxyPort=8080
  systemProp.https.proxyHost=127.0.0.1
  systemProp.https.proxyPort=8080
  ```

### 5. **`gradle-wrapper.properties (Gradle version)`**

- **作用**：

  - 定义Gradle Wrapper的配置，指定项目使用的Gradle版本。
  - 确保所有开发者和构建环境使用统一的Gradle版本，避免因版本差异导致的构建问题。

- **常见内容**：

  - 指定Gradle的版本号。
  - 配置Gradle分发的URL。

- **示例**：

  ```properties
  # gradle-wrapper.properties 文件
  distributionBase=GRADLE_USER_HOME
  distributionPath=wrapper/dists
  zipStoreBase=GRADLE_USER_HOME
  zipStorePath=wrapper/dists
  distributionUrl=https\://services.gradle.org/distributions/gradle-7.0.2-all.zip
  ```

### 6. **`local.properties (SDK Location)`**

- **作用**：

  - 定义本地开发环境的配置，主要用于指定Android SDK的路径。
  - 该文件通常由Android Studio自动生成，不应提交到版本控制系统中。

- **常见内容**：

  - 指定`sdk.dir`路径。

- **示例**：

  ```properties
  # local.properties 文件
  sdk.dir=/Users/username/Library/Android/sdk
  ```

### 总结

这些文件在Android项目中各自承担着不同的职责，合理配置这些文件可以确保项目的构建、依赖管理和开发环境的一致性。