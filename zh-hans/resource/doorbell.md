## 低功耗门铃

### 休眠唤醒

低功耗门铃由电池供电，为了节省电量，在一定时间内没有 p2p 连接会休眠，休眠后无法直接连接 p2p，需要先唤醒设备，再连接 p2p 通道。

**接口说明**

```java
void wirelessWake(String localKey, String devId);
```

**示例代码**

```java
String mDevId = devBean.getDevId();
String mLocalkey = devBean.getLocalKey();
mTuyaCameraDevice.wirelessWake（mDevId,mLocalkey）;
```

### 电池管理

低功耗门铃有两种供电方式，插电和电池供电。通过 Camera SDK 可以查询到设备当前的供电模式以及当前的电量。还可以设置一个低电量报警阈值，当电量过低时，会触发一个报警通知。


#### 电池锁的数据下发

**示例代码**

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



#### 电池及设备状态信息的数据下发

**示例代码**

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

> 注意：DpWirelessElectricity数据下发不需要带参数，所以传null值即可。



#### 低电量报警阈值的数据下发

**示例代码**

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



#### 设备供电方式的数据下发

**示例代码**

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

> 注意：DpWirelessPowermode 数据下发不需要带参数，所以传null值即可。

