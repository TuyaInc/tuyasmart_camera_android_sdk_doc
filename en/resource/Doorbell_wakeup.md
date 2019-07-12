# Low power doorbell wakeUp


Wake-up of Door Bell and Operating Method of Link


```java
mDeviceControl = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
//wake up
mDeviceControl.wirelessWake(localkey, devId); 
//connect
camera.connect(p2pId, p2pWd,localKey);
```

The flow chart is as follows:

![](./images/wakeup_flow.png)