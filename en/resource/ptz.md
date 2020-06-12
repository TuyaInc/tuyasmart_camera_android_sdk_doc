## PTZ control


Tuya PTZ camera can be remotely controlled to rotate in the specified direction through Tuya Smart Camera Android SDK.

> When the SDK controls the pan / tilt head to rotate, it is not a unit angle rotation. After the SDK issues a rotation start command, the device will rotate uniformly in the specified direction until it fails to rotate or receives a command to stop rotation.

Before starting to control the PTZ camera rotation, you need to determine whether the current device supports PTZ control.

**Example**


```java
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpPTZControl.ID, new ITuyaCameraDeviceControlCallback<String>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String o) {
    showPublishTxt.setText("LAN/Cloud query result: " + o);
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
mTuyaCameraDevice.publishCameraDps(DpPTZControl.ID, PTZDirection.LEFT);//云台向左转动
mTuyaCameraDevice.publishCameraDps(DpPTZStop.ID, null); // 停止云台转动
```
