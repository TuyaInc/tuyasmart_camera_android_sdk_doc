# 低功耗门铃唤醒

门铃的唤醒及链接的操作方法

```java
int crcsum = CRC32.getChecksum(mDeviceBean.getLocalKey().getBytes());
String topicId = "m/w/" + mDeviceBean.getDevId();
byte[] bytes = IntToButeArray.intToByteArray(crcsum);
ITuyaHomeCamera homeCamera = TuyaHomeSdk.getCameraInstance();
//wake up
homeCamera.publishWirelessWake(topicId, bytes); 
//connect
camera.connect(p2pId, p2pWd,localKey);
```



流程图如下：

![](./images/wakeup_flow.png)



