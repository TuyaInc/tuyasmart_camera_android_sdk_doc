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



