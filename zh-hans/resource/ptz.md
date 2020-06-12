## 云台控制

云台摄像机可以通过 Camera SDK 远程控制其向指定方向转动。

> Camera SDK 控制云台机转动时，并不是单位角度转动。Camera SDK 下发开始转动的命令后，设备会朝着指定方向持续转动，直到无法转动，或者收到停止转动的命令。

在开始控制云台摄像机转动前，需要先判断当前设备是否支持云台控制。

**示例代码**

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