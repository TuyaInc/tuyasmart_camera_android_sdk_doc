# 低功耗门铃唤醒


##门铃的唤醒及链接的操作方法

###门铃的唤醒方法

```java
void wirelessWake(String localkey, String devId); 
```
#### 参数说明

| 参数    | 说明                               |
| ------- | ---------------------------------- |
| localkey | 设备 localkey |
| devId | 设备id |

###示例代码

```java
mDeviceControl = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
//wake up
mDeviceControl.wirelessWake(localkey, devId); 
//connect
camera.connect(p2pId, p2pWd,localKey);
```





流程图如下：

![](./images/wakeup_flow.png)

