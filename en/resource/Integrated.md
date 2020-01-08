# Integrate SDK



## Preparation for Integration


###  1、Create Project

Build your project in the Android Studio.


### 2、Configure the build.gradle

build.gradle 

Add the following codes to the build.gradle file.

```groovy
defaultConfig {
    ndk {
        abiFilters "armeabi-v7a"
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

      //Mall components
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

> 【Tips】 TuyaSmart Camera Android sdk solely supports the platform of armeabi-v7a



### 3、Set the AndroidManifest.xml

Set appkey and appSecret in the AndroidManifest.xml file, and configure corresponding permissions, etc.

```xml
<meta-data
android:name="TUYA_SMART_APPKEY"
android:value="App id" />
<meta-data
android:name="TUYA_SMART_SECRET"
android:value="App key" />

<!-- sdcard -->
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<!-- Network -->
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
```



### 4、proguard-rules

Arrange aliasing configuration in corresponding proguard-rules.pro files.

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

