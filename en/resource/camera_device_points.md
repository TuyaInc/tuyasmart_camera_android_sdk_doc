
# Camera function data-points

Tuya smart device controls the device through the device function point, and realizes the interaction between the device and the app through the standardized function point. **Tuya Smart Camera Android SDK** is based on [custom device control](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/Device.html) and encapsulates a set of extended functions for smart cameras.



## Camera device standardized features

### Device basic features setting

| Data-point | Data type | value | Description |            Feature definition |
| ------ | ------ | :----: | ------ | ------ |
| DpBasicIndicator.ID | boolean |                                         True： turn on indicator under normal state                                 False: turn off indicator under normal state | Status indicator | Turn on/off device indicator under normal state: avoid light pollution at night. Indicator needs to be turned on during wifi configuration process or device is abnormal. |
| DpBasicFlip.ID | boolean |                 True: flip               False: normal status                 | Screen flip |                          Turn on/off screen vertical flip: adjust screen direction in the case when the device is upside down. |
| DpBasicOSD.ID | boolean | True: OSD appears         False: OSD disappears              |  Time watermark |  Turn on/off time watermark in live view mode |
| DpBasicPrivate.ID | boolean | True: private mode turned on             False: private mode turned off | Private mode | Turn on/off private mode: no video&audio stream collecting, the device has no live view and playback |
| DpBasicNightvision.ID | enum | Auto, 1: Off, 2: On |  IR night vision | Select IR night vision status On/Off/Auto |



### Motion detection alarm

| Data-point     | Data type | value                                                        | Description                                              | Feature definition |
| ---------------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| DpPIRSwitch.ID         | enum                | 0：off, 1: low sensitivity, 2: medium sensitivity, 3: high sensitivity | PIR switch and sensitivity          |  Adjust PIR status: Off/low sensitivity/medium sensitivity/ high sensitivity |
| DpMotionSwitch.ID      | boolean             | True: motion detection alarm turned on               False: motion detection alarm turned off | Motion detection alarm switch               |  Turn on/off motion detection alarm. After turning on: device needs to report messages to server once movement is detected under suitable conditions. |
| DpMotionSensitivity.ID | enum                |  0: low,1:medium, 2:high             |  motion detection sensitivity setting |   Set motion detection alarm sensitivity |



### Sound detection alarm

| Data-point       |  Data type | value                                                        |      Description                                        |                                Feature definition |
| ----------------------- | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| DpDecibelSwitch.ID      | boolean               |   True: sound detection alarm turned on              False: sound detection alarm turned off |  Sound detection alarm switch               |  Turn on/off sound detection alarm. After turning on: device needs to report messages to server once sound is detected under suitable conditions. |
| DpDecibelSensitivity.ID | enum                  | low,1:medium, 2:high |  sound detection sensitivity setting | Set sound detection alarm sensitivity |



### Memory card and local video recording management

|  Data-point  |   Data type | value                                                        |  Description                                            |              Feature definition                     |
| ------------------- | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| DpSDStatus.ID       | value                 |  1: normal, 2: abnormal (SD card is broken or under wrong format), 3: insufficient space, 4: formatting, 5: no SD card inserted |  Memory card status                              | Device reports current SD card status to the cloud, including SD card status: normal /abnormal /insufficient space etc. |
| DpSDStorage.ID      | string                | string                                                       |   Memory card capacity info research     |  Report memory card current capacity status/ capacity used/ capacity left |
| DpSDFormat.ID       | boolean               | boolean                                                      |       Memory card formatting                    | Initial formatting instruction, report current memory card formatting process |
| DpSDFormatStatus.ID | value                 |              Positive number of formatting process 0-100 |   current memory card formatting process |      Positive number of formatting process: 100 means formatting success.  Error: -2000: SD card is formatting, -2001: SD card formatting |
| DpSDRecordSwitch.ID | boolean               | True: local video recording turned on        False: local video recording turned off |  local video recording switch                  | Trun on/off SD card record: device will not record video and save to SD card after turning off the switch. |
| DpRecordMode.ID     | enum                  |                                1: event record (start to record once movement is detected)                             2: continuous record |   local video recording mode select    |   select local video recording mode, support event record and continuous record. |



### PTZ functions

| Data-point |  Data type | value                                                        |  Description                      |      Feature definition           |
| ----------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------- | ------------------------------------------------------------ |
| DpPTZControl.ID   | enum                |             0: left, 2: right, 4: down, 6: up |   PTZ direction  control |                                        Use to control the PTZ, and adjust direction |
| DpPTZStop.ID      | boolean             |         bool(PTZ stop moving, no parameters) | Stop PTZ         |   PTZ stop moving, no parameters. |



### Battery powered product features

|  Data-point       |  Data type | value                                                        | Description                                      |                          Feature definition       |
| ------------------------ | -------------------- | ------------------------------------------------------------ | ---------------------------------------------------- | ------------------------------------------------------------ |
| DpWirelessElectricity.ID | value                |     Device power, report 0-100 integer |    Battery and device status info |   Device reports current battery power, and reports when power changes. Device sends messages to cloud current device sleep/wake status. |
| DpWirelessLowpower.ID    | value                | value                                                        |          Low power alarm value       |  Users can set the value in App. Alarm regulation: dp145≤dp147 |
| DpWirelessBatterylock.ID | boolean              |       True: locked    False: unlocked |    Battery lock                           |                  Turn on/off battery lock control |
| DpWirelessPowermode.ID   | enum                 |   0: Battery power supply, 1: Plug power supply |      Device power supply method    |      Device reports current power supply status. Report once power supply method changes. |



## Device standardized feature method 


ITuyaCameraDevice provides device info communication ability, control orders issue, reach current data-points info



### Obtain objective

```java
//initaling device by device ID
ITuyaCameraDevice tuyaCameraDevice = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
```

**description**

Initialize the device control class based on the device id.

```java
ITuyaCameraDevice getCameraDeviceInstance(String devId) 
```

**Sample code**

```java
ITuyaCameraDevice tuyaCameraDevice = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
```




#### Data-point support or not


**【Description】**


Judge whether the device supports the data-point. Call data issues/data research will fail if the device dose not support certain data-point. 

```java
boolean isSupportCameraDps(String dpCodeID);
```

**Sample code**

```java
boolean isSupportDpBasicFlip = mTuyaCameraDevice.isSupportCameraDps(DpBasicFlip.ID);
```



#### Data publish

**【Description】**

Send message through LAN or cloud

**Sample code**

```java
//Callback is able to set as Boolean/String/Integer when the data-point type is Boolean/Enum/Value.
mTuyaCameraDevice.publishCameraDps(DpBasicFlip.ID, true, null, new ITuyaCameraDeviceControlCallback<Boolean>() {
  @Override
  public void onSuccess(String s, DpNotifyModel.ACTION action, 													DpNotifyModel.SUB_ACTION sub_action, Boolean o) {
    Log.d("device control", "value " + action + "   " + sub_action + " o " + 						o );
  }

  @Override
  public void onFailure(String s, DpNotifyModel.ACTION action, 											DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

  }
});
```





#### ITuyaCameraDeviceControlCallback


 ITuyaCameraDeviceControlCallback provides listener device info data receiving, and receive device callback  after data publishing from App side. Callback is able to set as Boolean/String/Integer when the data-point type is Boolean/Enum/Value.

```java
public interface ITuyaCameraDeviceControlCallback<E> {
    //Callback succeed
    void onSuccess(String devId, ACTION action, SUB_ACTION subAction, E o);
	//Callback fail
    void onFailure(String devId, ACTION action, SUB_ACTION subAction, String errorCode, String errorString);
}
```

Supported data types：

| data type | Description  |
| --------- | ------------ |
| Boolean   | Boolean      |
| String    | String, Enum |
| Value     | Number       |


#### 

### Value data-point research

**description**

IGet the data of the corresponding function point through the cache. According to the data type of Value, you can use the following methods to query.

```java
 int queryIntegerCurrentCameraDps(String dpCodeID);
```

**Sample code**

```java
int dpvalue = mTuyaCameraDevice.queryIntegerCurrentCameraDps(DpSDStatus.ID);
```



### Support all data-point research

**description**

Obtain the data of corresponding function points through the cache, and support all function point queries of enum, value, boolean, and String.

```java
Object queryObjectCameraDps(String dpCodeID);
```

**Sample code**

```java
Object dpValue = mTuyaCameraDevice.queryObjectCameraDps(DpBasicFlip.ID);
```

> If you use: queryObjectCameraDps for querying, developers need to distinguish the data types separately.



### Support String/Enum data-point research

**description**

Get the data of the corresponding function point through the cache. According to the data type of String / Enum, you can use the following methods to query.

```java
String queryStringCurrentCameraDps(String dpCodeID);
```

**Sample code**

```java
String mode = mTuyaCameraDevice.queryStringCurrentCameraDps(DpMotionSensitivity.ID);
```



### Support boolean data-point research

**description**

Get the data of the corresponding function point through the cache. According to the data type of boolean, you can use the following methods to query.

```java
boolean queryBooleanCameraDps(String dpCodeID);
```

**Sample code**

```java
boolean dpValue = mTuyaCameraDevice.queryBooleanCameraDps(DpBasicFlip.ID);
```



### Enumerated Function 

The value range of the function point of the string enumeration type. There are corresponding string enumeration constants defined in the SDK.

| Function             | Enum                  |
| -------------------- | --------------------- |
| DpMotionSensitivity  | MotionSensitivityMode |
| DpBasicNightvision   | NightStatusMode       |
| DpPIRSwitch          | PIRMode               |
| DpRecordMode         | RecordMode            |
| DpPTZControl         | PTZDirection          |
| DpDecibelSensitivity | SoundSensitivityMode  |



## Memory card and local video recording management

**description**

Before starting to manage the memory card or perform video playback, you need to obtain the status of the memory card. If the device does not detect the memory card, you cannot proceed to the next step. If the memory card is abnormal, you need to format the memory card first.

>  sdcard LAN/Cloud data publish dose not need other parameters



### DpSDStatus data publish

**Sample code**

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



### DpSDStorge data publish

  Precondition: confirm whether the data-point&sdcard are exist before data is published 

**Sample code**

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



### DpSDFormat data publish

When formatting the memory card, there are two cases according to the implementation of the camera manufacturer. The firmware implemented by some manufacturers will actively report the progress of formatting, and will also actively report the current capacity status after formatting is completed. However, there are a few manufacturers' firmware that will not actively report, so it is necessary to periodically and actively check the format Progress, when the progress reaches 100, then actively query the current capacity status. DpSDFormat data delivery can start the format operation.

**Sample code**

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



### DpSDFormatStatus data publish

DpSDFormatStatus data can be used to query the formatting progress. When the progress reaches 100, the formatting ends. You can check the memory card capacity again.

**Sample code**

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



### DpSDRecordSwitch data publish

The data of DpSDRecordSwitch is sent to control whether the Tuya smart camera has the recording function enabled.

**Sample code**

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



### DpSDRecordModel data publish

After the Tuya camera is inserted into the memory card, the captured image recording can be saved in the memory card, and the video recording switch and mode can be set through the Tuya IPC Camera SDK. There are two recording modes:

- Continuous recording: The camera will continuously record the collected audio and video on the memory card. When the capacity of the memory card is insufficient, the oldest recorded video data will be overwritten.
- Event recording: The camera will only start recording video when the detection alarm is triggered. The length of the video will change according to the type of event and the duration of the event.

The data of DpSDRecordModel is sent to set the recording mode.

**Sample code**

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



## Low-power doorbell features

### Wake-up feature data publish

```java
void wirelessWake(String localKey, String devId);
```

**Sample code**

```java
String mDevId = devBean.getDevId();
String mLocalkey = devBean.getLocalKey();
mTuyaCameraDevice.wirelessWake（mDevId,mLocalkey）;
```



### Battery lock data publish

**Sample code**

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



### Batter and device status info data publish

**Sample code**

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



### Low battery alarm value data publish

**Sample code**

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



### Power supply method data publish

**Sample code**

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



## PTZ Controll

Tuya PTZ camera can be remotely controlled to rotate in the specified direction through Tuya Smart Camera Android SDK.

> When the SDK controls the pan / tilt head to rotate, it is not a unit angle rotation. After the SDK issues a rotation start command, the device will rotate uniformly in the specified direction until it fails to rotate or receives a command to stop rotation.

Before starting to control the PTZ camera rotation, you need to determine whether the current device supports PTZ control.

**Sample code**

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

 