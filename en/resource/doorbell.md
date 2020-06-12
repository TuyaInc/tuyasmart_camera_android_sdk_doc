## Low power doorbell

### Sleep and wake

Low power doorbell is powered by battery. In order to save power, camera will sleep when no p2p connection for a certain period of time. After sleeping, it cannot be directly connected to p2p. You need to wake up the device, and then connect to the p2p channel after waking up.

**Declaration**

```java
void wirelessWake(String localkey, String devId);
```

**Parameter**

| Parameter |   Description   |
| --------- | --------------- |
| localkey  | Device localkey |
| devId     | device Id       |

**Example**

```java
mDeviceControl = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
//wake up
mDeviceControl.wirelessWake(localkey, devId); 
//connect
camera.connect(p2pId, p2pWd,localKey);
```

### Battery management

There are two ways to power low-power doorbells, plug-in and battery-powered. The SDK can query the current power supply mode and current power of the device. You can also set a low battery alarm threshold. When the battery is too low, an alarm notification will be triggered.

#### Battery lock data publish

**Example**


```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpWirelessBatterylock.ID, new ITuyaCameraDeviceControlCallback<Boolean>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, Boolean o) {
    showPublishTxt.setText("LAN/Cloud query result: " + o);
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
mTuyaCameraDevice.publishCameraDps(DpWirelessBatterylock.ID, true);
```


#### Batter and device status info data publish

**Example**


```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpWirelessElectricity.ID, new ITuyaCameraDeviceControlCallback<Integer>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, Integer o) {
    showPublishTxt.setText("LAN/Cloud query result: " + o);
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
mTuyaCameraDevice.publishCameraDps(DpWirelessElectricity.ID, null);
```

> Tip: DpWirelessElectricity data publish only needs null value (dose not require parameters). 



#### Low battery alarm value data publish

**Example**


```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpWirelessLowpower.ID, new ITuyaCameraDeviceControlCallback<Integer>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, Integer o) {
    showPublishTxt.setText("LAN/Cloud query result: " + o);
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
mTuyaCameraDevice.publishCameraDps(DpWirelessLowpower.ID, 20);
```



#### Power supply method data publish

**Example**


```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpWirelessPowermode.ID, new ITuyaCameraDeviceControlCallback<String>() {
                @Override
                public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String o) {
                    showPublishTxt.setText("LAN/Cloud query result: " + o);
                }

                @Override
                public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

                }
            });
mTuyaCameraDevice.publishCameraDps(DpWirelessPowermode.ID, null);
```

> Tip: DpWirelessPowermode data publish only needs null value (dose not require parameters). 

