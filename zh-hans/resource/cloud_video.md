# 云存储

涂鸦平台为智能摄像机提供云存储的服务，可以将设备录制的视频上传到涂鸦云端。

## 流程图

先获取云存储服务状态，如果云存储服务未开通或者已经过期并且云视频已经被全部删除（云存储服务过期后，已经上传的云视频还会保留一段时间，通常是 7 天），就需要先购买云存储服务。如果云存储服务在有效期，先获取有云存储视频的日期，然后获取指定日期的相关数据，包括云存储事件，时间轴数据，鉴权信息等。之后就可以选择一个云存储事件或者一个时间点开始播放云视频了。

![](./images/cloud_storage_process.png)

## 服务购买

云存储服务购买需要使用云存储购买 H5 组件，该组件提供了云存储购买的 H5 页面和订单展示功能

### 组件引入

项目根目录的 build.gradle：

```groovy
buildscript {
    ···
    dependencies {
        classpath 'com.android.tools.build:gradle:3.1.4'
        classpath 'com.tuya.android.module:tymodule-config:0.4.0-SNAPSHOT'
    }
}

```

项目模块的 build.gradle：

```groovy
apply plugin: 'tymodule-config'

...
//商城组件
implementation 'com.tuya.smart:tuyasmart-webcontainer:3.12.6r125'
implementation 'com.tuya.smart:tuyasmart-xplatformmanager:1.1.0'
implementation "com.tuya.smart:tuyasmart-base:3.13.0r127"
implementation 'com.tuya.smart:tuyasmart-appshell:3.10.0'
implementation "com.tuya.smart:tuyasmart-stencilwrapper:3.13.0r127"
implementation "com.tuya.smart:tuyasmart-framework:3.13.0r127-open-rc.1"
implementation 'com.tuya.smart:tuyasmart-uispecs:0.0.3'
```

### 使用方法

**示例代码**

Styles.xml 需要修改

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
<!-- 普通的文本按钮 -->
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

<!-- listview有分割线 -->
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
  <!-- 此处需要 android-14 -->
  <item name="android:textAllCaps">false</item>
</style>

<!-- 普通文本 -->
<style name="TextView.Normal" parent="android:Widget.TextView">
  <item name="android:textSize">@dimen/ts_15</item>
  <item name="android:textColor">@color/textColor</item>
  <item name="android:lineSpacingExtra">4dp</item>
  <item name="android:ellipsize">end</item>
</style>
<!-- 普通加载 -->
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


<!--横线-->
<style name="line_long">
  <item name="android:layout_width">match_parent</item>
  <item name="android:layout_height">@dimen/line_dip</item>
  <item name="android:background">@color/list_line_color</item>
</style>
<style name="dialog_style" parent="android:Animation">
  <item name="android:windowEnterAnimation">@anim/dialog_enter</item>
  //进入时的动画
  <item name="android:windowExitAnimation">@anim/dialog_exit</item>
  //退出时的动画
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

<!-- 设置项表单单项 -->
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

<!--横线-->
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
<!-- 文本输入框 -->
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
  <!-- 菜单标题文字颜色 -->
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

application 初始化

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

### 跳转到云存储购买页面

```java
findViewById(R.id.buy_btn).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //购买页面
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

## 云视频


### 云存储数据

#### 集成准备

项目模块的 build.gradle：

```groovy

implementation 'com.tuya.smart:tuyasmart-ipc-camera-middleware:3.14.3r133'
```

**CameraCloudSDK ** 是云存储数据的 API 接口调用的实体对象，提供了获取云存储购买，时间数据相关的 API 方法。


#### 获取云存储当前状态值（有无购买等）

```java
void getCameraCloudInfo(DeviceBean deviceBean, ICloudCacheManagerCallback callback)
```
**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| deviceBean | 设备信息 |
| callback | 回调方法|

**示例代码**

```java
cameraCloudSDK.getCameraCloudInfo(TuyaHomeSdk.getDataInstance().getDeviceBean(devId), CameraCloudStorageActivity.this);
```



#### 获取设备云存储相关的数据 Secret、Auth 等参数

```java
public void getCloudMediaCount(String devId, String timeZone, ICloudCacheManagerCallback callback)
```
**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| devId | 设备id |
| timeZone | 时区|
| callback | 回调方法|

**示例代码**

```java
cameraCloudSDK.getCloudMediaCount(devId, TimeZone.getDefault().getID(), CameraCloudStorageActivity.this);
```



#### 获取指定时间的时间片

```java
public void getTimeLineInfoByTimeSlice(String devId, String timeGT, String timeLT, ICloudCacheManagerCallback callback)
```
**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| devId | 设备id |
| timeGT | 开始时间（13位时间戳）|
| timeLT | 结束时间|
| callback | 回调方法|

**示例代码**

```java
getTimeLineInfoByTimeSlice(devId, String.valueOf(dayBean.getCurrentStartDayTime()), String.valueOf(dayBean.getCurrentDayEndTime()));
```



#### 根据时间片段始末获取相应的移动侦测数据

```java
public void getMotionDetectionByTimeSlice(String devId, String timeGT, String timeLT, int offset, int limit, ICloudCacheManagerCallback callback) 
```

**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| devId | 设备id |
| timeGT | 开始时间（13位时间戳）|
| timeLT | 结束时间|
| offset | 第几页，默认0|
| limit | 每次拉取条数，默认-1，表示所有数据|
| callback | 回调方法|

**示例代码**

```java
cameraCloudSDK.getMotionDetectionByTimeSlice(devId, timeGT, timeLT, offset, limit, this);
```



#### 云存储购买地址接口

```java
public void buyCloudStorage(Context mContext, DeviceBean deviceBean, String homeId, ICloudManagerCallback callback) {
        CameraCloudManager.getInstance().getCloudStorageUrl(mContext, deviceBean, homeId);
    }
```

**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| mContext | 上下文 |
| deviceBean | 设备信息）|
| homeId | 家庭id|
| callback | 回调方法|

**示例代码**

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



#### 销毁

```java
public void onDestroy()
```

**示例代码**

```java
if (null != cameraCloudSDK) {
  cameraCloudSDK.onDestroy();
}
```



#### 云存储回调方法

**接口说明**

调用 sdk 之后的回调方法

```java
public interface ICloudCacheManagerCallback {

    /**
     * 返回有云存储数据日期
     *
     * @param cloudDayBeanList 云存储日期数据集
     */
    void getCloudDayList(List<CloudDayBean> cloudDayBeanList);

    /**
     * 返回 云存储 encryKey
     *
     * @param encryKey
     */
    void getCloudSecret(String encryKey);

    /**
     * 返回 云存储 authorityJson
     *
     * @param authorityJson 云存储秘钥
     */
    void getAuthorityGet(String authorityJson);

    /**
     * 返回 云存储 根据某一天时间段查询的数据
     *
     * @param timePieceBeans 时间片段数据集
     */
    void getTimePieceInfoByTimeSlice(List<TimePieceBean> timePieceBeans);

    /**
     * 返回 云存储-移动侦测 根据某一天时间段查询的数据
     *
     * @param timeRangeBeans 移动侦测时间片段数据集
     */
    void getMotionDetectionByTimeSlice(List<TimeRangeBean> timeRangeBeans);

    /**
     * 错误码
     *
     * @param errorCode
     */
    void onError(int errorCode);

    /**
     * 返回 云存储 状态
     *
     * @param code 购买状态，code值参考文章底下的状态码
     */
    void getCloudStatusSuccess(int code);

    /**
     * 返回 云存储 配置信息，需要传入sdk验证
     *
     * @param config 配置信息数据
     */
    void getCloudConfigDataTags(String config);
}

```

##### 错误回调

**接口说明**

```java
void onError(int errorCode);
```
**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| errorCode | 错误码 |



##### 状态值

| 状态值    | 说明                               |
| :------- | :---------------------------------- |
| 10001 | 未开通服务 |
| 10002 | 已开通服务，无数据 |
| 10003 | 已开通服务，有数据 |
| 10004 | 服务已过期，有数据 |
| 10005 | 服务已过期，无数据 |



##### 错误码

| 错误码    | 说明                               |
| :------- | :---------------------------------- |
| 10100 | 获取 SECRET 失败 |
| 10101 | 获取 AUTH 失败 |
| 10110 | 其他异常 |
| 10010 | 获取认证信息失败 |
| 10011 | 获取某天的时间片（鉴权信息）失败 |



### 云存储播放

**ITYCloudCamera** 提供了云存储播放相关的API接口

#### 初始化

创建设备对象

```java
void createCloudDevice(String cachePath, String devId)
```

**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| cachePath | 缓存路径 |
| devId | 设备id |

**示例代码**

```java
cloudCamera.createCloudDevice(cachePath, devId);
```


#### 注册云存储监听

```java
void registorOnP2PCameraListener(OnP2PCameraListener listener);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| listener | p2p 回调 |

**示例代码**

```java
cloudCamera.registorOnP2PCameraListener(this);
```



#### 反注册云存储监听

```java
void removeOnP2PCameraListener();
```
**示例代码**

```java
cloudCamera.removeOnP2PCameraListener();
```



#### 绑定播放组件 view

```java
void generateCloudCameraView(IRegistorIOTCListener view);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| view | 播放器组件 |

**示例代码**

```java
cloudCamera.generateCloudCameraView(mVideoView);
```



#### 配置云存储数据 tags (开始播放云存前需要先进行配置)

```java
int configCloudDataTagsV1(String jsonStr, OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| jsonStr | 配置云存储数据 tags |
| callBack | 回调 |

**示例代码**

```java
cloudCamera.configCloudDataTagsV1(config, new OperationDelegateCallBack() {

  @Override
  public void onSuccess(int i, int i1, String s) {
    //成功之后开始播放
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


#### 播放云存储视频

```java
void playCloudDataWithStartTime(long mStartTime, long mEndTime, boolean isEvent, String jsonAuthParams, String encryptKey, OperationCallBack callback, OperationCallBack playFinishedCallBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| mStartTime | 开始时间 |
| mEndTime | 结束时间，一般是这一天的结束时间 |
| isEvent | 是否是侦测事件，默认false |
| jsonAuthParams | 校验数据|
| encryptKey | 秘钥 |
| callBack | 播放回调 |
| playFinishedCallBack | 播放结束回调 |

**示例代码**

```java
cloudCamera.playCloudDataWithStartTime(startTime, endTime, isEvent,
                                       mAuthorityJson, mEncryptKey,
                                       new OperationCallBack() {
                                         @Override
                                         public void onSuccess(int sessionId, int requestId, String data, Object camera) {
                                           // 播放中的回调, playing
                                         }

                                         @Override
                                         public void onFailure(int sessionId, int requestId, int errCode, Object camera) {

                                         }
                                       }, new OperationCallBack() {
                                         @Override
                                         public void onSuccess(int sessionId, int requestId, String data, Object camera) {
                                           //播放完成的回调, playCompleted
                                         }

                                         @Override
                                         public void onFailure(int sessionId, int requestId, int errCode, Object camera) {
                                         }
                                       });
```



##### 暂停播放

```java
int pausePlayCloudVideo(OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| callBack | 操作回调 |

**示例代码**

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



##### 继续播放

```java
int resumePlayCloudVideo(OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| callBack | 操作回调 |

**示例代码**

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



##### 停止播放

```java
int stopPlayCloudVideo(OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| callBack | 操作回调 |

**示例代码**

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


#### 其他功能

云存储视频播放也提供有声音开关，本地视频录制，截图等功能。

##### 设置静音状态

``` java
void setCloudMute(final int mute, OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| mute | 是否静音 |
| callBack | 操作回调 |

**示例代码**

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


##### 获取静音状态

```java
int getCloudMute();
```
**示例代码**

```java
cloudCamera.getCloudMute()
```


##### 截图

```java
int snapshot(String absoluteFilePath, OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| absoluteFilePath | 文件地址 |
| callBack | 操作回调 |

**示例代码**

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



##### 开始录制

```java
int startRecordLocalMp4(String folderPath, String fileName, OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| folderPath | 文件夹路径 |
| fileName | 文件名称 |
| callBack | 操作回调 |

**示例代码**

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



##### 结束录制

```java
int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| callBack | 操作回调 |

**示例代码**

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



#### 反初始化

```java
void deinitCloudCamera();
```

**示例代码**

```java
cloudCamera.deinitCloudCamera();
```