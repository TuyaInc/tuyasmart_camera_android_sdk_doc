# DecryptImageView



## Introduction

The image decryption component DecryptImageView is an image loading component developed based on Fresco. Due to the tuya security mechanism, the image display requires data decryption through a custom imagepipeline-okhttp3.

## Import

Add dependencies of build.gradle under the module:

```groovy
implementation 'com.tuya.smart:tuyasmart-imagepipeline-okhttp3:0.0.1'
implementation 'com.facebook.fresco:fresco:1.3.0'
```



## Method



### init

```java
public static void initFresco(Context context);
```



### load Image

```java
public void setImageURI(String uriString, byte[] key)
```

**Parameter Description**

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| uriString | corresponding to the p2p capability of the corresponding device |
| key       | Key data                                                     |



### Sample Code

DecryptImageView is an image loading component based on [Fresco](https://www.fresco-cn.org/docs/index.html).

1. Before the image is loaded, it is first initialized. It only needs to be loaded once by FrescoManager.initFresco (this), which is most suitable for application.

   ```java
   FrescoManager.initFresco(this);
   ```

   > Note: FrescoManager.initFresco (this) does not need to call Fresco.initialize (this), otherwise the picture cannot be decrypted.

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

   After doing the above, you need to specify your Application class in AndroidManifest.xml. In order to download network images, please confirm that you have declared the permissions requested by the network.

2. show Image

   In the xml layout file, add the namespace:

   ```xml
   <LinearLayout
       xmlns:android="http://schemas.android.com/apk/res/android"
       xmlns:fresco="http://schemas.android.com/apk/res-auto"
       android:layout_height="match_parent"
       android:layout_width="match_parent">
   ```

   add DecryptImageView:

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

3. Load image

   Start loading encrypted images, pass in the decryption key decryption, imageUrl, and call mSnapshot.setImageURI (imageUrl, decryption.getBytes ()).

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

   

   