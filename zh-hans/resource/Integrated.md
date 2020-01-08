# 集成SDK



## 集成准备



###  一、创建工程

在Android Studio中建立你的工程。


### 二、build.gradle配置

在工程根目录的build.gradle 文件里添加如下配置

```groovy
	buildscript {

    repositories {
        ...
        maven {
            url 'https://raw.githubusercontent.com/TuyaInc/mavenrepo/master/releases'
        }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.1.4'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        ...
        maven {
            url 'https://raw.githubusercontent.com/TuyaInc/mavenrepo/master/releases'
        }
    }
}

```

app目录下的build.gradle配置：

```groovy
defaultConfig {
    ndk {
        abiFilters "armeabi-v7a","arm64-v8a"
    }
 }
    dependencies {
        implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])
        implementation 'com.alibaba:fastjson:1.1.67.android'
        implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'
        implementation 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'
     
	    // required tuya home sdk
	    implementation 'com.tuya.smart:tuyasmart:3.12.4'
	     
	    // tuya camera module
    	
    	implementation 'com.tuya.smart:tuyasmart-ipc-camera-middleware:3.14.3r133'
		implementation 'com.tuya.smart:tuyasmart-ipc-camera-v2:3.14.4r134'
		implementation 'com.tuya.smart:tuyasmart-ipc-camera-utils:3.13.0r128'
		implementation 'com.tuya.smart:tuyasmart-ipc-camera-message:3.13.0r128'
		implementation 'com.tuya.smart:tuyasmart-ipc-devicecontrol:3.14.3r133'

    	//商城组件
      implementation 'com.tuya.smart:tuyasmart-webcontainer:3.12.6r125'
      implementation 'com.tuya.smart:tuyasmart-xplatformmanager:1.0.0'
      implementation "com.tuya.smart:tuyasmart-base:3.13.0r127"
      implementation 'com.tuya.smart:tuyasmart-appshell:3.10.0'
      implementation "com.tuya.smart:tuyasmart-stencilwrapper:3.13.0r127"
      implementation "com.tuya.smart:tuyasmart-framework:3.13.0r127-open-rc.1"
      implementation 'com.tuya.smart:tuyasmart-uispecs:0.0.3'
    }

repositories {
    mavenLocal()
    jcenter()
    google()
}
```

> 【注意事项】 涂鸦智能摄像机sdk默认支持armeabi-v7a,arm64-v8a



### 三、AndroidManifest.xml 设置

在AndroidManifest.xml文件里配置appkey和appSecret，在配置相应的权限等

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



### 四、混淆配置

在proguard-rules.pro文件配置相应混淆配置

```bash
#fastJson
-keep class com.alibaba.fastjson.**{*;}
-dontwarn com.alibaba.fastjson.**

#mqtt
-keep class org.eclipse.paho.client.mqttv3.** { *; }
-dontwarn org.eclipse.paho.client.mqttv3.**

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

