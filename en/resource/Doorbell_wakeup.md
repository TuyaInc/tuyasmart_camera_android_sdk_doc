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

# Door Bell Message
When the device doorbell is pressed, the device will push a message to the App. The current information is pushed from the public version of the push information. If the message body carries **type=doorbell**, it can be judged as the doorbell push message.

```xml
	{"a":"view","c":"action","cc":"***** ,someone is ringing the bell.","ct":"fcm You have a visitor","devId":"6cfaf335a8d6e752e0wrpy","msgId":"4da4dcf61573555995","p":{"media":13},"specialChannel":false,"ts":"1573555995000","type":"doorbell"}
```