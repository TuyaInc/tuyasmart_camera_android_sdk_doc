#  集成 SDK


## 对接方式

在接入 Camera SDK 之前，您可以先了解一下 [Home SDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/)，Camera SDK 需要依赖 Home SDK 其中的一部分，下面的文档也会介绍到依赖的这一部分。

涂鸦平台提供多种对接方案，开发者可以根据自己的需求选择不同的对接方式，可以参考[方案介绍](https://docs.tuya.com/zh/iot/open-api/quick-start/solution-overview)。

不同的对接方案需要集成的 SDK 不同。


## 准备工作


1. 在涂鸦 iot 平台中“ App 工作台”中点击 App SDK，点击“获取 SDK ”

<img src="https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/images/getsdk.png" style="zoom:67%;" />

2. 根据需求选择合适的开发方案，点击“下一步”。

![](./images/sdk_preparation_1.png)

3. 根据提示输入创建的 app 信息，点击”下一步“。

![](./images/sdk_preparation_3.png)

4. 在安卓部分可以获取 `AppKey`、`AppSecret`。点击"下载安全图片"，“ Android 下载（Gradle）”下载所需要的安全图片和依赖包信息。

![](./images/sdk_preparation_2.png)

	

##  创建工程

在 Android Studio 中建立你的工程。



### build.gradle 配置



1. 在工程根目录的 build.gradle 文件里添加如下配置

```groovy
buildscript {

    repositories {
       ...
       maven {
           url 'https://maven-other.tuya.com/repository/maven-releases/'
       }
       maven {
           url 'https://maven-other.tuya.com/repository/maven-snapshots/'
       }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.1.4'
            classpath 'com.tuya.android.module:tymodule-config:0.4.0-SNAPSHOT'
        }
    }

allprojects {
    repositories {
        ...
        maven {
            url 'https://maven-other.tuya.com/repository/maven-releases/'
        }
        maven {
            url 'https://maven-other.tuya.com/repository/maven-snapshots/'
        }
    }
}
   
```

2. app 目录下的 build.gradle 配置：

```groovy
apply plugin: 'tymodule-config'

defaultConfig {
   ndk {
       abiFilters "armeabi-v7a","arm64-v8a"
   }
}
dependencies {
   implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])
   implementation 'com.alibaba:fastjson:1.1.67.android'
   implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'
   // implementation 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'

   // required tuya home sdk
   implementation 'com.tuya.smart:tuyasmart:3.17.0-beta1'

   // tuya camera module
   implementation 'com.tuya.smart:tuyasmart-ipc-camera-middleware:3.14.3r133'
   implementation 'com.tuya.smart:tuyasmart-ipc-camera-v2:3.17.0r139'
   implementation 'com.tuya.smart:tuyasmart-ipc-camera-utils:3.13.0r129h1'
   implementation 'com.tuya.smart:tuyasmart-ipc-camera-message:3.13.0r128'
   implementation 'com.tuya.smart:tuyasmart-ipc-devicecontrol:3.17.0r139'

   //messge center imagepipeline 
   implementation 'com.tuya.smart:tuyasmart-imagepipeline-okhttp3:0.0.1'
   implementation 'com.facebook.fresco:fresco:1.3.0'

}

repositories {
   mavenLocal()
   jcenter()
   google()
}
```

> 【注意事项】 涂鸦智能摄像机sdk默认支持 armeabi-v7a , arm64-v8a



### AndroidManifest.xml 设置

在 AndroidManifest.xml 文件里配置 `appkey` 和 `appSecret` ，再配置相应的权限等

```xml
<meta-data
android:name="TUYA_SMART_APPKEY"
android:value="应用id" />
<meta-data
android:name="TUYA_SMART_SECRET"
android:value="应用密钥" />

<!-- sdcard -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<!-- 网络 -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
```



### 混淆配置

在 proguard-rules.pro 文件配置相应混淆配置

```bash
#fastJson
-keep class com.alibaba.fastjson.**{*;}
-dontwarn com.alibaba.fastjson.**

#mqtt
-keep class com.tuya.smart.mqttclient.mqttv3.** { *; }
-dontwarn com.tuya.smart.mqttclient.mqttv3.**

#tutk
-keep class com.tutk.**{*;}
-dontwarn com.tutk.**

-keep class com.squareup.okhttp.** { *; }
-keep interface com.squareup.okhttp.** { *; }
-dontwarn com.squareup.okhttp.**

-keep class okio.** { *; }
-dontwarn okio.**

-keep class com.tuya.**{*;}
-dontwarn com.tuya.**
-keep class com.tuyasmart.**{*;}
-dontwarn com.tuyasmart.**

```

