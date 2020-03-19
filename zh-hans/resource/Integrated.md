# 集成SDK



**Tuya Smart Camera SDK ** 依赖于**涂鸦全屋智能 APP SDK ** ，在接入涂鸦 **Tuya Smart Camera SDK** 之前，您务必先了解一下 [涂鸦全屋智能 APP SDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/)。



##  一、创建工程

在 Android Studio 中建立你的工程。



## 二、build.gradle 配置



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
	
	        // NOTE: Do not place your application dependencies here; they belong
	        // in the individual module build.gradle files
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
	
	    }
	
	repositories {
	    mavenLocal()
	    jcenter()
	    google()
	}
	```

> 【注意事项】 涂鸦智能摄像机sdk默认支持 armeabi-v7a , arm64-v8a



## 三、AndroidManifest.xml 设置

在 AndroidManifest.xml 文件里配置 `appkey` 和 `appSecret` ，在配置相应的权限等

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



## 四、混淆配置

在 proguard-rules.pro 文件配置相应混淆配置

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

