# 低功耗门铃唤醒

门铃的唤醒及链接的操作方法

```java
mDeviceControl = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
//wake up
mDeviceControl.wirelessWake(localkey, devId); 
//connect
camera.connect(p2pId, p2pWd,localKey);
```



流程图如下：

![](./images/wakeup_flow.png)




# 按门铃接收推送消息
当按下设备门铃时，设备端会推送消息至App。推送目前的信息来源于公版的推送信息，其中如果消息体中携带有type=doorbell，即可判断为门铃推送消息
```xml
	{"a":"view","c":"action","cc":"低功耗智能摄像机 ,someone is ringing the bell.","ct":"fcm You have a visitor","devId":"6cfaf335a8d6e752e0wrpy","msgId":"4da4dcf61573555995","p":{"media":13},"specialChannel":false,"ts":"1573555995000","type":"doorbell"}
```
