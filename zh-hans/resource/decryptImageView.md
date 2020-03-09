# 图片解密组件-DecryptImageView



> 简介：图片解密组件DecryptImageView是基于Fresco开发的图片加载组件，因tuya安全机制，所以图片显示需要通过自定义imagepipeline-okhttp3进行数据解密。



## 引用
模块下的build.gradle的依赖添加：

```xml
//消息中心图片解密组件
implementation 'com.tuya.smart:tuyasmart-imagepipeline-okhttp3:0.0.1'
implementation 'com.facebook.fresco:fresco:1.3.0'
```



## 方法

### 初始化

```java
public static void initFresco(Context context);
```

### 加载图片

```java
public void setImageURI(String uriString, byte[] key)
```

#### 参数说明

| 参数      | 说明                               |
| --------- | ---------------------------------- |
| uriString | p2p类型，对应匹配相应设备的p2p能力 |
| key       | 秘钥数据                           |



## 使用方法

  DecryptImageView是基于[Fresco](https://www.fresco-cn.org/docs/index.html)开发的图片加载组件

1. 在图片加载之前，首先初始化，只需要FrescoManager.initFresco(this)一次加载就可以，在application中做再合适不过。

   ```java
   FrescoManager.initFresco(this);
   ```

   > 注意：FrescoManager.initFresco(this)就不需要调用Fresco.initialize(this)，不然无法进行图片解密。

   ```java
   public class TuyaSmartApp extends MultiDexApplication {
   
       private static final String TAG = "TuyaSmartApp";
   
       @Override
       public void onCreate() {
           super.onCreate();
           ...
           FrescoManager.initFresco(this);
           ...
       }
     ...
   ```

   

   做完上面的工作后，你需要在 `AndroidManifest.xml` 中指定你的 Application 类。为了下载网络图片，请确认你声明了网络请求的权限。

2. 图片显示

   在xml布局文件中, 加入命名空间：

   ```xml
   <LinearLayout
       xmlns:android="http://schemas.android.com/apk/res/android"
       xmlns:fresco="http://schemas.android.com/apk/res-auto"
       android:layout_height="match_parent"
       android:layout_width="match_parent">
   ```

   加入DecryptImageView：

   ```xml
   <com.tuya.drawee.view.DecryptImageView
           android:id="@+id/iv_time_range_snapshot"
           android:layout_below="@+id/rv_lv_oval"
           android:layout_width="match_parent"
           android:layout_height="@dimen/dp_159"
           android:layout_marginLeft="@dimen/dp_48"
           android:layout_marginTop="@dimen/dp_15"
           android:layout_marginRight="@dimen/dp_45"
           fresco:actualImageScaleType="fitXY"
           fresco:roundedCornerRadius="@dimen/dp_4" />
   ```

   开始加载加密图片，传入解密秘钥**decryption**，**imageUrl**，调用mSnapshot.setImageURI(imageUrl, decryption.getBytes())。

   ```java
   String attachPics = cameraMessageBean.getAttachPics();
               mSnapshot.setVisibility(View.VISIBLE);
               if (attachPics.contains("@")) {
                   int index = attachPics.lastIndexOf("@");
                   try {
                       String decryption = attachPics.substring(index + 1);
                       String imageUrl = attachPics.substring(0, index);
                       mSnapshot.setImageURI(imageUrl, decryption.getBytes());
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
               } else {
                   Uri uri = null;
                   try {
                       uri = Uri.parse(attachPics);
                   } catch (Exception e) {
                       e.printStackTrace();
                   }
                   DraweeController controller = Fresco.newDraweeControllerBuilder().setUri(uri).build();
                   mSnapshot.setController(controller);
               }
   ```

   