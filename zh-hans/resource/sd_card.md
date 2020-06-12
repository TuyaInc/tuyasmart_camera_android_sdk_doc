## 存储卡管理

存储卡管理通过设备功能点实现，相关的功能点请在[设备功能点](./camera_device_points.md)中查看。



### 状态 

在开始管理存储卡或者进行录像回放前，需要先获取存储卡的状态，如果设备未检测到存储卡，则无法进行下一步。如果存储卡异常，则需要先格式化存储卡。

DpSDStatus 是控制 sdcard 状态的 DP 点。

> sdcard 的 LAN/Cloud 数据下发是不需要传其他参数的。

**示例代码**

```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpSDStatus.ID, new ITuyaCameraDeviceControlCallback<Integer>() {
    @Override
    public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, Integer o) {
        showPublishTxt.setText("LAN/Cloud query result: " + o);
    }

    @Override
    public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

    }
});
mTuyaCameraDevice.publishCameraDps(DpSDStatus.ID, null);
```

### 格式化 

在格式化存储卡的时候，根据摄像机厂商的实现，有两种情况。有些厂商实现的固件中，会主动上报格式化的进度，格式化完成后也会主动上报当前的容量状态，但是有少部分厂商的固件，不会主动上报，所以需要定时主动去查询格式化的进度，当进度达到 100 时，再主动去查询当前的容量状态。DpSDFormat 的数据下发可以启动格式化操作。

**示例代码**

```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpSDFormat.ID, new ITuyaCameraDeviceControlCallback<Boolean>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, Boolean o) {
    showPublishTxt.setText("LAN/Cloud query result: " + o);
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
mTuyaCameraDevice.publishCameraDps(DpSDFormat.ID, true);
```

### 格式化状态

DpSDFormatStatus 数据下发可以查询格式化进度，当进度达到100时，即格式化结束。可以再次去查询存储卡容量。

**示例代码**

```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpSDFormatStatus.ID, new ITuyaCameraDeviceControlCallback<Integer>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, Integer o) {
    showPublishTxt.setText("LAN/Cloud query result: " + o);
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
mTuyaCameraDevice.publishCameraDps(DpSDFormatStatus.ID, null);
```

### 存储卡录制


涂鸦摄像机在插入存储卡后，可以将采集的影像录制保存在存储卡中，可以通过 SDK 设置视频录制开关和模式。录制模式分为以下两种：

* **连续录制**：摄像机会将采集到的音视频连续不断的录制保存在存储卡中，存储卡的容量不足的时候，将会覆盖最早录制的视频数据。
* **事件录制**：摄像机只会在触发侦测报警的时候才会开始录制视频，视频的长短会根据事件类型，和事件持续时间而变化。

DpSDRecordModel 的数据下发来设置录像模式。

**示例代码**

```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpSDRecordModel.ID, new ITuyaCameraDeviceControlCallback<String>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String o) {
    showPublishTxt.setText("LAN/Cloud query result: " + o);
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
mTuyaCameraDevice.publishCameraDps(DpSDRecordModel.ID, RecordMode.EVENT.getDpValue());
```

### 存储卡容量获取 

DpSDStorge 的数据下发可以获取到涂鸦 IPC 摄像机当前的存储卡容量。

**示例代码**

```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpSDStorage.ID, new ITuyaCameraDeviceControlCallback<String>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String o) {
    showPublishTxt.setText("LAN/Cloud query result: " + o);
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
mTuyaCameraDevice.publishCameraDps(DpSDStorage.ID, null);
```

> 存储卡容量值的字符串格式：总容量|已使用容量|空闲容量，单位`KB`



### 录像开关

DpSDRecordSwitch 的数据下发来控制涂鸦智能摄像机是否开启录像功能。

**示例代码**

```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpSDRecordSwitch.ID, new ITuyaCameraDeviceControlCallback<Boolean>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, Boolean o) {
    showPublishTxt.setText("LAN/Cloud query result: " + o);
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
mTuyaCameraDevice.publishCameraDps(DpSDRecordSwitch.ID, true);
```

```

