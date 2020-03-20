# 消息推送



## 简介

**Tuya Smart Camera Android SDK **消息推送使用的是 **涂鸦智能全屋SDK** 的消息推送配置和接收，具体请 [参阅](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/MessagePush.html)



## 按门铃接收推送消息

当按下设备门铃时，设备端会推送消息至 App。推送目前的信息:

可以从[系统的推送信息](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/MessagePush.html)中获取，其中如果消息体中携带有 **"type"："doorbell"**，即可判断为门铃推送消息。参考如下：

```json
{
	"a": "view",
	"c": "action",
	"cc": "低功耗智能摄像机 ,someone is ringing the bell.",
	"ct": "fcm You have a visitor",
	"devId": "6cfaf335a8d6e752e0wrpy",
	"msgId": "4da4dcf61573555995",
	"p": {
		"media": 13
	},
	"specialChannel": false,
	"ts": "1573555995000",
	"type": "doorbell"
}
```



## 消息推送辅助协议

消息推送辅助协议可以提高消息的到达率和及时性

> 注意：前提是 APP 进程处于 active 状态



### 使用方法



#### registerCameraPushListener

在登录 tuya 账号后，注册 registerCameraPushListener ，调用方法：

```java
 void registerCameraPushListener(ITuyaGetBeanCallback<CameraPushDataBean> callback);
```

**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| ITuyaGetBeanCallback | 收到消息数据的回调 |



#### ITuyaGetBeanCallback 

消息回调接口

**方法说明**

| 方法     | 说明     |
| :-------- | :-------- |
| onResult | 回调结果 |



#### CameraPushDataBean

 消息体

**参数说明**

| 参数      | 描述       |
| --------- | ---------- |
| timestamp | 消息时间戳 |
| devid     | 设备 id    |
| msgid     | 消息 id    |
| etype     | 消息类型   |

> Note: 门铃来电消息的 etype=doorbell 



#### unRegisterCameraPushListener

在退出 tuya 账号后，反注册，调用 unRegisterCameraPushListener

```java
void unRegisterCameraPushListener(ITuyaGetBeanCallback<CameraPushDataBean> callback);
```

**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| ITuyaGetBeanCallback | 收到消息数据的回调 |

> 反注册方法参数的 callback 与注册的 callback 是同一个



#### 示例代码

 ```java
   package com.tuya.smart.android.demo.base.utils;
   
   import android.app.Activity;
   import android.content.Context;
   import android.content.Intent;
   import android.os.Bundle;
   
   import com.tuya.smart.android.camera.api.ITuyaHomeCamera;
   import com.tuya.smart.android.camera.api.bean.CameraPushDataBean;
   import com.tuya.smart.android.common.utils.L;
   import com.tuya.smart.android.demo.R;
   import com.tuya.smart.android.demo.login.activity.LoginActivity;
   import com.tuya.smart.android.demo.base.app.Constant;
   import com.tuya.smart.api.MicroContext;
   import com.tuya.smart.home.sdk.TuyaHomeSdk;
   import com.tuya.smart.sdk.api.ITuyaGetBeanCallback;
   import com.tuyasmart.stencil.utils.BaseActivityUtils;
   
   import static com.tuya.smart.camera.utils.IntentUtils.INTENT_CONTENT;
   import static com.tuya.smart.camera.utils.IntentUtils.INTENT_DEVID;
   import static com.tuya.smart.camera.utils.IntentUtils.INTENT_MSGID;
   import static com.tuya.smart.camera.utils.IntentUtils.INTENT_TITLE;
   
   
   /**
    * Created by letian on 16/7/15.
    */
   public class LoginHelper {
       private static final String TAG = "LoginHelper";
       private static ITuyaHomeCamera homeCamera;
   
       private static ITuyaGetBeanCallback<CameraPushDataBean> mTuyaGetBeanCallback = new ITuyaGetBeanCallback<CameraPushDataBean>() {
           @Override
           public void onResult(CameraPushDataBean o) {
               L.d(TAG, "onMqtt_43_Result on callback");
               L.d(TAG, "timestamp=" + o.getTimestamp());
               L.d(TAG, "devid=" + o.getDevId());
               L.d(TAG, "msgid=" + o.getEdata());
               L.d(TAG, "etype=" + o.getEtype());
   
           }
       };
   
       public static void afterLogin() {
   
           //there is the somethings that need to set.For example the lat and lon;
           //   TuyaSdk.setLatAndLong();
           homeCamera = TuyaHomeSdk.getCameraInstance();
           if (homeCamera != null) {
               homeCamera.registerCameraPushListener(mTuyaGetBeanCallback);
           }
       }
   
       private static void afterLogout() {
           L.d(TAG, "afterLogout unregister thread " + Thread.currentThread().getName());
           if (homeCamera != null) {
               homeCamera.unRegisterCameraPushListener(mTuyaGetBeanCallback);
           }
           homeCamera = null;
       }
   
       /**
        * 唤起重新登录
        *
        * @param context
        */
       public static void reLogin(Context context) {
           reLogin(context, true);
       }
   
       public static void reLogin(Context context, boolean tip) {
           onLogout(context);
           if (tip) {
               ToastUtil.shortToast(context, R.string.login_session_expired);
           }
           ActivityUtils.gotoActivity((Activity) context, LoginActivity.class, ActivityUtils.ANIMATE_FORWARD, true);
       }
   
       private static void onLogout(Context context) {
           afterLogout();
           exit(context);
       }
   
       /**
        * 退出应用
        *
        * @param context
        */
       public static void exit(Context context) {
           Constant.finishActivity();
           TuyaHomeSdk.onDestroy();
       }
   }
   
 ```

> Note: 当app进程被杀死的时候，该监听无效。当app登录成功之后，注册监听；app退出登录，取消监听