# Low power doorbell wakeUp


Wake-up of Door Bell and Operating Method of Link


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

