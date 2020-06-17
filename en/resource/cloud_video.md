# Cloud Storage

Tuya IoT provides cloud storage service for smart camera, which can upload the video recorded by the device to Tuya cloud.

## Flow chart


Request the cloud storage service status first. If the cloud storage service is not activated or has expired and the cloud video has been completely deleted (after the cloud storage service expires, the uploaded cloud video will be retained for some days), you need to first purchase cloud storage services. If the cloud storage service is in the validity period, first request the dates of the cloud storage video, and then request the relevant data of the specified date, including cloud storage events, timeline data, authentication information, etc. After that, you can choose a cloud storage event or a point in time to start playing cloud video.

![](./images/cloud_storage_process.png)

## Cloud service

This component provides H5 pages and order display functions for cloud storage purchases.

### Import

1. Build.gradle in the project root：

```groovy
buildscript {
    ···
    dependencies {
        classpath 'com.android.tools.build:gradle:3.1.4'
        classpath 'com.tuya.android.module:tymodule-config:0.4.0-SNAPSHOT'
    }
}

```

2. Build.gradle of the project module：

```groovy
apply plugin: 'tymodule-config'

...
implementation 'com.tuya.smart:tuyasmart-webcontainer:3.12.6r125'
implementation 'com.tuya.smart:tuyasmart-xplatformmanager:1.1.0'
implementation "com.tuya.smart:tuyasmart-base:3.13.0r127"
implementation 'com.tuya.smart:tuyasmart-appshell:3.10.0'
implementation "com.tuya.smart:tuyasmart-stencilwrapper:3.13.0r127"
implementation "com.tuya.smart:tuyasmart-framework:3.13.0r127-open-rc.1"
implementation 'com.tuya.smart:tuyasmart-uispecs:0.0.3'
```



### Use

**Example**

Styles.xml needs to be modified

```xml

<!-- Base application theme. -->
<style name="AppTheme" parent="Theme.AppCompat.NoActionBar">
  <!-- Customize your theme here. -->
  <item name="colorPrimary">@color/colorPrimary</item>
  <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
  <item name="colorAccent">@color/colorAccent</item>
  <item name="android:windowBackground">@color/window_bg</item>
</style>

<style name="SplashTheme" parent="AppTheme">
  <item name="android:windowBackground">@drawable/ty_pre</item>
</style>

<style name="edit_text_input">
  <item name="android:layout_width">match_parent</item>
  <item name="android:layout_height">@dimen/edit_height</item>
  <item name="android:background">@null</item>
  <item name="android:paddingRight">10dp</item>
  <item name="android:gravity">center_vertical</item>
  <item name="android:textCursorDrawable">@null</item>
  <item name="android:textSize">15sp</item>
  <item name="android:singleLine">true</item>
  <item name="android:textColorHint">?attr/list_secondary_color</item>
  <item name="android:textColor">?attr/list_primary_color</item>
</style>

<style name="TY_SingleLine_Normal">
  <item name="android:layout_height">1px</item>
  <item name="android:layout_width">match_parent</item>
  <item name="android:background">@color/line_color</item>
</style>

<style name="Button.Normal" parent="android:Widget.Button">
  <item name="android:layout_height">@dimen/wh_36</item>
  <item name="android:minWidth">@dimen/wh_88</item>
  <item name="android:layout_width">wrap_content</item>
  <item name="android:singleLine">true</item>
  <item name="android:textSize">@dimen/ts_18</item>
  <item name="android:textStyle">normal</item>
  <item name="android:ellipsize">end</item>
  <item name="android:background">@drawable/button_orange</item>
  <item name="android:textColor">@color/button_orange_text</item>
</style>

<style name="TY.Progress.Dialog" parent="Theme.AppCompat.Dialog">
  <item name="colorAccent">@color/color_accent</item>
  <item name="android:lineSpacingExtra">@dimen/wh_4</item>
</style>


<style name="TY_List_Normal">
  <item name="android:fadingEdge">none</item>
  <item name="android:listSelector">@android:color/transparent</item>
  <item name="android:divider">@color/line_color</item>
  <item name="android:dividerHeight">@dimen/single_pix</item>
  <item name="android:listDivider">@drawable/ty_color_line</item>
  <item name="android:footerDividersEnabled">false</item>
  <item name="android:cacheColorHint">@android:color/transparent</item>
  <item name="android:scrollbarThumbVertical">@drawable/ty_scrollbar_thumb</item>
  <item name="android:scrollbarTrackVertical">@null</item>
</style>


<style name="toolbar_menu_text" parent="Theme.AppCompat.Light.DarkActionBar">
  <item name="android:textSize">17sp</item>
  <item name="android:textStyle">bold</item>
</style>


<style name="AppTheme.Base" parent="Theme.AppCompat.NoActionBar">
  <!-- colorPrimary is used for the default action bar background -->
  <item name="colorPrimary">?attr/status_bg_color</item>
  <!-- colorPrimaryDark is used for the status bar -->
  <item name="colorPrimaryDark">?attr/status_bg_color</item>
  <!-- colorAccent is used as the default value for colorControlActivated,
              which is used to tint widgets -->
  <item name="colorAccent">@color/color_accent</item>

  <item name="android:textColorPrimary">@color/white</item>
  <item name="android:windowBackground">@drawable/ty_pre</item>


  <item name="android:windowContentOverlay">@android:color/transparent</item>

  <item name="android:windowFullscreen">false</item>

  <!-- You can also set colorControlNormal, colorControlActivated
            colorControlHighlight, and colorSwitchThumbNormal. -->
  <!--<item name="actionButtonStyle">@color/colorMenu</item>-->
  <!--<item name="android:actionButtonStyle">@color/colorMenu</item>-->
  <item name="android:colorBackgroundCacheHint">@android:color/transparent</item>

  <item name="android:popupBackground">@color/color_primary</item>

  <item name="android:windowAnimationStyle">@style/noAnimation</item>

  <item name="android:actionMenuTextColor">@color/white</item>
  <item name="android:actionMenuTextAppearance">@style/toolbar_action_text</item>
  <item name="actionModeBackground">@color/color_primary</item>

  <item name="colorSwitchThumbNormal">@color/gray</item>
  <item name="android:colorForeground">@color/gray_70</item>

  <item name="actionMenuTextColor">?attr/status_font_color</item>
</style>
<style name="noAnimation">
  <item name="android:activityOpenEnterAnimation">@null</item>
  <item name="android:activityOpenExitAnimation">@null</item>
  <item name="android:activityCloseEnterAnimation">@null</item>
  <item name="android:activityCloseExitAnimation">@null</item>
  <item name="android:taskOpenEnterAnimation">@null</item>
  <item name="android:taskOpenExitAnimation">@null</item>
  <item name="android:taskCloseEnterAnimation">@null</item>
  <item name="android:taskCloseExitAnimation">@null</item>
  <item name="android:taskToFrontEnterAnimation">@null</item>
  <item name="android:taskToFrontExitAnimation">@null</item>
  <item name="android:taskToBackEnterAnimation">@null</item>
  <item name="android:taskToBackExitAnimation">@null</item>
</style>

<style name="MyMenuTextAppearance" parent="android:TextAppearance.Holo.Widget.ActionBar.Menu">

  <item name="android:textAllCaps">false</item>
</style>


<style name="TextView.Normal" parent="android:Widget.TextView">
  <item name="android:textSize">@dimen/ts_15</item>
  <item name="android:textColor">@color/textColor</item>
  <item name="android:lineSpacingExtra">4dp</item>
  <item name="android:ellipsize">end</item>
</style>

<style name="ProgressBar.Normal" parent="Widget.AppCompat.ProgressBar">
  <item name="android:minWidth">23dp</item>
  <item name="android:maxWidth">23dp</item>
  <item name="android:minHeight">23dp</item>
  <item name="android:maxHeight">23dp</item>
  <item name="android:textColor">@color/color_626262</item>
</style>


<!-- list view-->
<style name="list_view">
  <item name="android:layout_width">match_parent</item>
  <item name="android:layout_height">wrap_content</item>
  <item name="android:background">?attr/list_bg_color</item>
  <item name="android:divider">@null</item>
  <item name="android:dividerHeight">0dp</item>
  <item name="android:clickable">true</item>
  <item name="android:descendantFocusability">blocksDescendants</item>
</style>



<style name="line_long">
  <item name="android:layout_width">match_parent</item>
  <item name="android:layout_height">@dimen/line_dip</item>
  <item name="android:background">@color/list_line_color</item>
</style>
<style name="dialog_style" parent="android:Animation">
  <item name="android:windowEnterAnimation">@anim/dialog_enter</item>

  <item name="android:windowExitAnimation">@anim/dialog_exit</item>

</style>

<style name="dialog_alert" parent="@android:style/Theme.Holo.DialogWhenLarge">
  <item name="android:windowNoTitle">true</item>
  <item name="android:windowBackground">@android:color/transparent</item>
  <item name="android:windowFrame">@null</item>
</style>

<style name="SampleTheme" parent="android:Theme">
  <item name="numberPickerStyle">@style/NPWidget.Holo.NumberPicker</item>
</style>
<style name="NPWidget.Holo.NumberPicker" parent="NPWidget.NumberPicker">
  <item name="solidColor">@android:color/transparent</item>
  <item name="selectionDivider">@null</item>
  <item name="selectionDividerHeight">1dip</item>
  <item name="internalLayout">@layout/number_picker_with_selector_wheel</item>
  <item name="internalMinWidth">64dip</item>
  <item name="internalMaxHeight">180dip</item>
</style>

<style name="NPWidget.NumberPicker">
  <item name="android:orientation">vertical</item>
  <item name="android:fadingEdge">vertical</item>
  <item name="android:fadingEdgeLength">50dip</item>
</style>
<style name="NPWidget">
  <item name="android:textAppearance">?android:attr/textAppearance</item>
</style>


<style name="ListItem.ItemNormal" parent="android:Widget">
  <item name="android:layout_height">48dp</item>
  <item name="android:layout_width">match_parent</item>
  <item name="android:paddingLeft">16dp</item>
  <item name="android:paddingRight">16dp</item>
  <item name="android:paddingTop">@dimen/ts_12</item>
  <item name="android:paddingBottom">@dimen/ts_12</item>
  <item name="android:gravity">center_vertical|right</item>
  <item name="android:textColor">@color/text_color</item>
</style>


<style name="Dialog.Alert.NoTitle" parent="@style/Dialog.Alert">
  <item name="android:windowNoTitle">true</item>
</style>

<style name="Dialog.Alert" parent="Theme.AppCompat.Light.Dialog.Alert">
  <item name="colorAccent">@color/color_accent</item>
  <item name="android:lineSpacingExtra">@dimen/wh_4</item>
</style>

<style name="Dialog.Alert.Multichoice" parent="@style/Dialog.Alert">
  <!--<item name="android:listChoiceIndicatorMultiple">@null</item>-->
</style>

<style name="Dialog.Alert.Singlechoice" parent="@style/Dialog.Alert">
  <!--<item name="android:listChoiceIndicatorSingle">@null</item>-->
</style>

<style name="ToolBarStyle" parent="ToolBarStyle.Base"/>

<style name="ToolBarStyle.Base" parent="">
  <item name="popupTheme">@style/ThemeOverlay.AppCompat.Light</item>
  <item name="theme">@style/ThemeOverlay.AppCompat.Dark.ActionBar</item>
  <item name="titleTextAppearance">@style/myActionTitleTextAppearance</item>
</style>


<style name="line_normal">
  <item name="android:layout_width">match_parent</item>
  <item name="android:layout_height">@dimen/line_dip</item>
  <item name="android:background">@color/line</item>
</style>

<style name="myActionTitleTextAppearance"
       parent="@style/TextAppearance.AppCompat.Widget.ActionBar.Menu">
  <item name="android:textSize">@dimen/tuya_title_size</item>
  <item name="android:textStyle">normal</item>
  <item name="android:textColor">@color/white</item>
  <item name="android:textAllCaps">false</item>
</style>

<style name="EditText.Normal" parent="android:Widget.EditText">
  <item name="android:background">@drawable/bg_white_round</item>
  <item name="android:textSize">@dimen/ts_16</item>
  <item name="android:textColor">@color/text_color</item>
  <item name="android:textColorHint">@color/text_hint_color</item>
  <item name="android:layout_height">wrap_content</item>
  <item name="android:layout_width">match_parent</item>
  <item name="android:gravity">center_vertical|left</item>
  <item name="android:singleLine">true</item>
  <item name="android:ellipsize">end</item>
  <item name="android:maxLength">64</item>
  <item name="android:textCursorDrawable">@drawable/cursor_orange</item>
</style>

<style name="Default_Public_Theme" parent="AppTheme">

  <item name="status_font_color">@color/app_bg_color</item>
  <item name="status_bg_color">@color/navbar_font_color</item>
  <item name="status_system_bg_color">@color/status_system_bg_color</item>
  <item name="navbar_font_color">@color/navbar_font_color</item>
  <item name="navbar_bg_color">@color/navbar_font_color</item>
  <item name="app_bg_color">@color/app_bg_color</item>
  <item name="fragment_bg_color">@color/app_bg_color</item>

  <item name="list_primary_color">@color/list_primary_color</item>
  <item name="list_sub_color">@color/list_sub_color</item>
  <item name="list_secondary_color">@color/list_secondary_color</item>
  <item name="list_line_color">@color/list_line_color</item>
  <item name="list_bg_color">@color/list_bg_color</item>
  <item name="notice_font_color">@color/notice_font_color</item>
  <item name="notice_bg_color">@color/notice_bg_color</item>

  <item name="primary_button_font_color">@color/primary_button_font_color</item>
  <item name="primary_button_bg_color">@color/primary_button_bg_color</item>
  <item name="secondary_button_font_color">@color/secondary_button_font_color</item>
  <item name="secondary_button_bg_color">@color/secondary_button_bg_color</item>

  <item name="app_name">@string/app_name</item>
  <item name="login_flow">0</item>
  <item name="package_mode">0</item>

  <item name="bg_normal_text_bt">@drawable/bg_text_bts</item>

  <item name="language_mode">0</item>
  <item name="android:windowBackground">@color/status_bg_color</item>
  <item name="android:windowIsTranslucent">false</item>
  <item name="actionBarSize">@dimen/abc_action_bar_default_height_material</item>
  <item name="is_add_device_help_get_from_native">false</item>
  <item name="is_scene_support">true</item>
</style>
<style name="ToolrTheme" parent="Default_Public_Theme">
  <item name="status_font_color">@color/white</item>
  <item name="status_bg_color">@color/color_ff5800</item>
  <item name="app_bg_color">@color/color_ff5800</item>
</style>
<style name="Splash.Theme" parent="Default_Public_Theme">
  <item name="android:windowBackground">@drawable/ty_pre</item>
</style>


<color name="color_ff5800">#ff5800</color>
```

application initialization

  ```java
public class TuyaSmartApp extends MultiDexApplication {

  private static final String TAG = "TuyaSmartApp";

  @Override
  public void onCreate() {
    super.onCreate();
    context=this;
    L.d(TAG, "onCreate " + getProcessName(this));
    L.setSendLogOn(true);
    TuyaWrapper.init(this);
    TuyaSdk.init(this);
    ...
  }

  ```

### Jump to cloud service purchase page

```java
findViewById(R.id.buy_btn).setOnClickListener(new View.OnClickListener() {
  @Override
  public void onClick(View v) {
    cameraCloudSDK.buyCloudStorage(CameraCloudStorageActivity.this,
                                   TuyaHomeSdk.getDataInstance().getDeviceBean(devId),
                                   String.valueOf(FamilyManager.getInstance().getCurrentHomeId()), new ICloudManagerCallback() {
                                     @Override
                                     public void onError(int i) {

                                     }

                                     @Override
                                     public void onSuccess(Object o) {
                                       String uri = (String) o;
                                       Intent intent = new Intent(CameraCloudStorageActivity.this, WebViewActivity.class);
                                       intent.putExtra("Uri",uri);
                                       startActivity(intent);
                                     }
                                   });
  }
});
```


## Cloud video

### Cloud storage data

#### Integration preparation 

Build.gradle of the project module:

```groovy
...
    implementation 'com.tuya.smart:tuyasmart-ipc-camera-middleware:3.14.3r133'
...
```

CameraCloudSDK is an entity object called by the API interface of cloud storage ATOP, and provides API methods related to obtaining cloud storage purchase and time data.


#### Get cloud storage current status value (with or without purchase, etc.)

```java
void getCameraCloudInfo(DeviceBean deviceBean, ICloudCacheManagerCallback callback)
```

**Parameter**


| Parameter  | Description |
| ---------- | ----------- |
| deviceBean | device info |
| callback   | Callback    |

**Example**


```java
cameraCloudSDK.getCameraCloudInfo(TuyaHomeSdk.getDataInstance().getDeviceBean(devId), CameraCloudStorageActivity.this);
```

#### Get device cloud storage related data, Secret, Auth and other parameters

```java
public void getCloudMediaCount(String devId, String timeZone, ICloudCacheManagerCallback callback)
```
**Parameter**


| Parameter | Description |
| --------- | ----------- |
| devId     | device Id   |
| timeZone  | time Zone   |
| callback  | Callback    |

**Example**


```java
cameraCloudSDK.getCloudMediaCount(devId, TimeZone.getDefault().getID(), CameraCloudStorageActivity.this);
```

#### Get time slice at specified time

```java
public void getTimeLineInfoByTimeSlice(String devId, String timeGT, String timeLT, ICloudCacheManagerCallback callback)
```

**Parameter**


| Parameter | Description                   |
| --------- | ----------------------------- |
| devId     | device Id                     |
| timeGT    | Start time (13-bit timestamp) |
| timeLT    | End Time                      |
| callback  | Callback                      |

**Example**


```java
getTimeLineInfoByTimeSlice(devId, String.valueOf(dayBean.getCurrentStartDayTime()), String.valueOf(dayBean.getCurrentDayEndTime()));
```

#### Get the corresponding motion detection data according to the beginning and end of the time segment

```java
public void getMotionDetectionByTimeSlice(String devId, String timeGT, String timeLT, int offset, int limit, ICloudCacheManagerCallback callback) 
```

**Parameter**


| Parameter | Description                                                 |
| --------- | ----------------------------------------------------------- |
| devId     | device Id                                                   |
| timeGT    | Start time (13-bit timestamp)                               |
| timeLT    | End Time                                                    |
| offset    | Page number, default 0                                      |
| limit     | Number of pulls at a time, default -1, which means all data |
| callback  | callback                                                    |

**Example**


```java
cameraCloudSDK.getMotionDetectionByTimeSlice(devId, timeGT, timeLT, offset, limit, this);
```


#### Cloud storage purchase address interface

```java
public void buyCloudStorage(Context mContext, DeviceBean deviceBean, String homeId, ICloudManagerCallback callback) {
  CameraCloudManager.getInstance().getCloudStorageUrl(mContext, deviceBean, homeId);
}
```

**Parameter**

| Parameter  | Description |
| ---------- | ----------- |
| mContext   | /           |
| deviceBean | device Info |
| homeId     | home Id     |
| callback   | callback    |

**Example**


```java
cameraCloudSDK.buyCloudStorage(CameraCloudStorageActivity.this,
                               TuyaHomeSdk.getDataInstance().getDeviceBean(devId),
                               String.valueOf(FamilyManager.getInstance().getCurrentHomeId()), new ICloudManagerCallback() {
                                 @Override
                                 public void onError(int i) {

                                 }

                                 @Override
                                 public void onSuccess(Object o) {
                                   String uri = (String) o;
                                   Intent intent = new Intent(CameraCloudStorageActivity.this, WebViewActivity.class);
                                   intent.putExtra("Uri",uri);
                                   startActivity(intent);
                                 }
                               });
```



#### Destroy

```java
public void onDestroy()
```

**Example**


```java
if (null != cameraCloudSDK) {
  cameraCloudSDK.onDestroy();
}
```



#### ICloudCacheManagerCallback

**Parameter**


Callback method after calling SDK

```java
public interface ICloudCacheManagerCallback {

    /**
     * return date with cloud storage data
     *
     * @param cloudDayBeanList: Cloud storage date List
     */
    void getCloudDayList(List<CloudDayBean> cloudDayBeanList);

    /**
     * return cloud storage encryKey
     *
     * @param encryKey: Cloud storage key
     */
    void getCloudSecret(String encryKey);

    /**
     * return authorityJson
     *
     * @param authorityJson: Cloud storage key
     */
    void getAuthorityGet(String authorityJson);

    /**
     * return data queried by cloud storage based on a certain time period.
     *
     * @param timePieceBeans: Time-slice data list
     */
    void getTimePieceInfoByTimeSlice(List<TimePieceBean> timePieceBeans);

    /**
     * return data from cloud storage motion detection queries based on a certain time period.
     *
     * @param timeRangeBeans: Motion detection time segment data list
     */
    void getMotionDetectionByTimeSlice(List<TimeRangeBean> timeRangeBeans);

    /**
     * error code
     *
     * @param errorCode
     */
    void onError(int errorCode);

    /**
     * return cloud storage status
     *
     * @param code: code value, refer to the status code at the bottom of the article.
     */
    void getCloudStatusSuccess(int code);

    /**
     * return cloud storage configuration information, need to pass in SDK verification
     *
     * @param config: Configuration information data
     */
    void getCloudConfigDataTags(String config);
}

```


#### Status value

| Status value | Description                             |
| ------------ | --------------------------------------- |
| 10001        | Not Opened                              |
| 10002        | Service opened, no data                 |
| 10003        | Service is available, data is available |
| 10004        | Service expired with data               |
| 10005        | Service expired, no data                |



#### Error code

| error code | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| 10100      | Get SECRET failed                                            |
| 10101      | Failed to get AUTH                                           |
| 10110      | Other exceptions                                             |
| 10010      | Failed to obtain authentication information                  |
| 10011      | Failed to get time slice (authentication information) for a certain day |

### Cloud video playback

**ITYCloudCamera** provides API interfaces related to cloud storage playback.


#### Initialize and create device

```java
void createCloudDevice(String cachePath, String devId)
```

**Parameter**


| Parameter | Description |
| --------- | ----------- |
| cachePath | Cache path  |
| devId     | device Id   |

**Example**


```java
cloudCamera.createCloudDevice(cachePath, devId);
```


#### Register for Cloud camera listening

```java
void registorOnP2PCameraListener(OnP2PCameraListener listener);
```

**Parameter**


| Parameter | Description |
| --------- | ----------- |
| listener  | Callback    |

**Example**


```java
cloudCamera.registorOnP2PCameraListener(this);
```

#### Unregister Cloud Camera listening


```java
void removeOnP2PCameraListener();
```

**Example**


```java
cloudCamera.removeOnP2PCameraListener(this);
```


#### Binder video View

```java
void generateCloudCameraView(IRegistorIOTCListener view);
```

**Parameter**


| Parameter | Description |
| --------- | ----------- |
| view      | Video view  |

**Example**


```java
cloudCamera.generateCloudCameraView(mVideoView);
```


#### Configure cloud storage data tags (requires configuration before starting to play cloud storage).

```java
int configCloudDataTagsV1(String jsonStr, OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter | Description                       |
| --------- | :-------------------------------- |
| jsonStr   | Configure cloud storage data tags |
| callBack  | Callback                          |

**Example**


```java
cloudCamera.configCloudDataTagsV1(config, new OperationDelegateCallBack() {

  @Override
  public void onSuccess(int i, int i1, String s) {
    if (timePieceBeans.size() > 0) {
      int startTime = timePieceBeans.get(0).getStartTime();
      playCloudDataWithStartTime(startTime, (int) (getTodayEnd(startTime * 1000L) / 1000) - 1, true);
    }
  }

  @Override
  public void onFailure(int i, int i1, int i2) {

  }
});
```


#### Cloud storage playing

```java
void playCloudDataWithStartTime(long mStartTime, long mEndTime, boolean isEvent, String jsonAuthParams, String encryptKey, OperationCallBack callback, OperationCallBack playFinishedCallBack);
```

**Parameter Description**

| Parameter            | Description                               |
| -------------------- | ----------------------------------------- |
| mStartTime           | Starting time                             |
| mEndTime             | End time, usually the end time of the day |
| isEvent              | Whether to detect events, default false   |
| jsonAuthParams       | Check data                                |
| encryptKey           | Secret key                                |
| callBack             | play callback                             |
| playFinishedCallBack | Play finish callback                      |

**Sample Code**

```java
cloudCamera.playCloudDataWithStartTime(startTime, endTime, isEvent,
                                       mAuthorityJson, mEncryptKey,
                                       new OperationCallBack() {
                                         @Override
                                         public void onSuccess(int sessionId, int requestId, String data, Object camera) {

                                         }

                                         @Override
                                         public void onFailure(int sessionId, int requestId, int errCode, Object camera) {

                                         }
                                       }, new OperationCallBack() {
                                         @Override
                                         public void onSuccess(int sessionId, int requestId, String data, Object camera) {

                                         }

                                         @Override
                                         public void onFailure(int sessionId, int requestId, int errCode, Object camera) {
                                         }
                                       });
```



#### Pause playing

```java
int pausePlayCloudVideo(OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter | Description |
| --------- | ----------- |
| callBack  | call back   |

**Example**

```java
cloudCamera.pausePlayCloudVideo(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
```


#### Resume playing

```java
int resumePlayCloudVideo(OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter | Description |
| --------- | ----------- |
| callBack  | call back   |

**Example**


```java
cloudCamera.resumePlayCloudVideo(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
```


#### Stop playing

```java
int stopPlayCloudVideo(OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter | Description |
| --------- | ----------- |
| callBack  | call back   |

**Example**


```java
cloudCamera.stopPlayCloudVideo(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
```


#### Get sound status

```java
int getCloudMute();
```

**Example**


```java
cloudCamera.getCloudMute()
```


#### Set sound status

``` java
void setCloudMute(final int mute, OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter | Description |
| --------- | ----------- |
| mute      | is mute     |
| callBack  | call back   |

**Example**


```java
cloudCamera.setCloudMute(mute, new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    soundState = Integer.valueOf(data);
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
  }
});
```


#### snapshot

```java
int snapshot(String absoluteFilePath, OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter        | Description |
| ---------------- | ----------- |
| absoluteFilePath | file path   |
| callBack         | call back   |

**Example**


```java
cloudCamera.snapshot(IPCCameraUtils.recordSnapshotPath(devId), new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    Toast.makeText(CameraCloudStorageActivity.this, "snapshot success", Toast.LENGTH_SHORT).show();
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
  }
});
```


### start Recording

```java
int startRecordLocalMp4(String folderPath, String fileName, OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter  | Description |
| ---------- | ----------- |
| folderPath | folder path |
| fileName   | file name   |
| callBack   | Call back   |

**Example**


```java
cloudCamera.startRecordLocalMp4(IPCCameraUtils.recordPath(devId), String.valueOf(System.currentTimeMillis()), new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    Toast.makeText(CameraCloudStorageActivity.this, "record start success", Toast.LENGTH_SHORT).show();
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
  }
});
```


#### Stop recording

```java
int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter | Description |
| --------- | ----------- |
| callBack  | call back   |

**Example**


```java
cloudCamera.stopRecordLocalMp4(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    Toast.makeText(CameraCloudStorageActivity.this, "record end success", Toast.LENGTH_SHORT).show();
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
  }
});
```



### Deinit

```java
void deinitCloudCamera();
```

**Example**


```java
cloudCamera.deinitCloudCamera();
```

