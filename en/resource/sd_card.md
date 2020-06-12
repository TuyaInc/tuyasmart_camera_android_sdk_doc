## Memory card management

Mmemory card management use data point, for related data point refer to [Data point id](./camera_device_points.md#Data point id).

### Status


Before starting to manage the memory card or perform video playback, you need to obtain the status of the memory card. If the device does not detect the memory card, you cannot proceed to the next step. If the memory card is abnormal, you need to format the memory card first.

>  sdcard LAN/Cloud data publish dose not need other parameters


**Example**


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

### Format


When formatting the memory card, there are two cases according to the implementation of the camera manufacturer. The firmware implemented by some manufacturers will actively report the progress of formatting, and will also actively report the current capacity status after formatting is completed. However, there are a few manufacturers' firmware that will not actively report, so it is necessary to periodically and actively check the format Progress, when the progress reaches 100, then actively query the current capacity status. DpSDFormat data delivery can start the format operation.

**Example**


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



### Format status

DpSDFormatStatus data can be used to query the formatting progress. When the progress reaches 100, the formatting ends. You can check the memory card capacity again.

**Example**


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

> Tip: DpSDFormatStatus data publish only needs null value (dose not require parameters). 

### Memory card recording

After the Tuya camera is inserted into the memory card, the captured image recording can be saved in the memory card, and the video recording switch and mode can be set through the SDK. There are two recording modes:

* **Continuous recording**: The camera will continuously record the collected audio and video recordings on the memory card. When the capacity of the memory card is insufficient, the oldest recorded video data will be overwritten.
* **Event recording**: The camera will only start recording video when the detection alarm is triggered. The length of the video will change according to the type of event and the duration of the event.

**Example**

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
### Get memory card capacity

Precondition: confirm whether the data-point&sdcard are exist before data is published

**Example**


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

> Tip: DpSDStorge data publish only needs null value (dose not require parameters). 


### Recording switch

The data of DpSDRecordSwitch is sent to control whether the Tuya smart camera has the recording function enabled.

**Example**


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
 