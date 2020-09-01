# Message push



## Introduction

**Tuya Smart Camera Android SDK** message push uses **Tuya Smart Home SDK** for message push configuration and reception. For details, please [refer to](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/MessagePush.html).



## Press the doorbell to receive  messages

When the device doorbell is pressed, the device will push a message to the App.It can be obtained from the system's push information. If the message body carries `"type"`: `"doorbell"`, it can be determined as a doorbell message. The reference is as follows:

```json
	{"a":"view","c":"action","cc":"someone is ringing the bell.","ct":"fcm You have a visitor","devId":"6cfaf335a8d6e752e0wrpy","msgId":"4da4dcf61573555995","p":{"media":13},"specialChannel":false,"ts":"1573555995000","type":"doorbell"}
```



## Message Push Assistant Protocol

Message push auxiliary protocol can improve message arrival rate and timeliness.

> Note: The premise is that the APP process is active.



### Use

After logging in to your tuya account, register registerCameraPushListener.

```java
void registerCameraPushListener(ITuyaGetBeanCallback<CameraPushDataBean> callback);
```

**Parameter Description**

| Parameter            | Description       |
| -------------------- | ----------------- |
| ITuyaGetBeanCallback | get data callback |



#### ITuyaGetBeanCallback

**Parameter Description**

| Method   | Description |
| -------- | ----------- |
| onResult | result      |



#### CameraPushDataBean

**Parameter Description**

| Parameter | Description        |
| --------- | ------------------ |
| timestamp | message time stamp |
| devid     | device Id          |
| msgid     | message Id         |
| etype     | message Type       |

> Note: etype = doorbell for doorbell  message



#### unRegisterCameraPushListener

After logging out of the tuya account, unregister and call unRegisterCameraPushListener

**Parameter Description**

| Parameter            | Description       |
| -------------------- | ----------------- |
| ITuyaGetBeanCallback | get data callback |

> The callback of the anti-register method parameter is the same as the registered callback



### Sample Code

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
        *
        * @param context
        */
       public static void exit(Context context) {
           Constant.finishActivity();
           TuyaHomeSdk.onDestroy();
       }
   }
   
```

> Note: When the app process is killed, the listener is invalid. When the app logs in successfully, register for monitoring; when the app logs out, cancel the monitoring.