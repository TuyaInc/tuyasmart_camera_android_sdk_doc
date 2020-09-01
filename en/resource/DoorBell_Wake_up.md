# DoorBell Wake up



## Introduction

The  doorbell is powered by battery. In order to save power, there will be no p2p connection to sleep for a certain period of time. After sleeping, it cannot be directly connected to p2p. You need to wake up the device by wireless wake-up through the device function point, and then connect to the p2p channel after waking up.



## Wake-up and link operation method



### Wake up

```java
void wirelessWake(String localkey, String devId);
```

**Parameter Description**

| Parameter | Description     |
| --------- | --------------- |
| localkey  | Device localkey |
| devId     | device Id       |



#### Sampe Code 

```java
mDeviceControl = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
//wake up
mDeviceControl.wirelessWake(localkey, devId); 
//connect
camera.connect(p2pId, p2pWd,localKey);
```



### Flow Chart



![](./images/wakeup_flow.png)

