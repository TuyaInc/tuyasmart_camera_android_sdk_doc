
# Extension Features

Tuya smart device controls the device through the device function point, and realizes the interaction between the device and the app through the standardized function point. **Tuya Smart Camera Android SDK** is based on [custom device control](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/Device.html) and encapsulates a set of extended functions for smart cameras.

## Device control

### Create instance

**Declaration**

initaling device by device ID.


```java
ITuyaCameraDevice getCameraDeviceInstance(String devId) 
```

**Example**

```java
ITuyaCameraDevice tuyaCameraDevice = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
```

### Query value

**Declaration**

Judge whether the device supports the data-point. Call data issues/data research will fail if the device dose not support certain data-point. 

```java
boolean isSupportCameraDps(String dpCodeID);
```

**Example**


```java
boolean isSupportDpBasicFlip = mTuyaCameraDevice.isSupportCameraDps(DpBasicFlip.ID);
```

#### Value data-point

**Declaration**


IGet the data of the corresponding function point through the cache. According to the data type of Value, you can use the following methods to query.

```java
 int queryIntegerCurrentCameraDps(String dpCodeID);
```

**Example**


```java
int dpvalue = mTuyaCameraDevice.queryIntegerCurrentCameraDps(DpSDStatus.ID);
```

#### Object data-point

**Declaration**


Obtain the data of corresponding function points through the cache, and support all function point queries of enum, value, boolean, and String.

```java
Object queryObjectCameraDps(String dpCodeID);
```

**Example**


```java
Object dpValue = mTuyaCameraDevice.queryObjectCameraDps(DpBasicFlip.ID);
```

> If you use: queryObjectCameraDps for querying, developers need to distinguish the data types separately.

#### String/Enum data-point

**Declaration**

Get the data of the corresponding function point through the cache. According to the data type of String / Enum, you can use the following methods to query.

```java
String queryStringCurrentCameraDps(String dpCodeID);
```

**Example**


```java
String mode = mTuyaCameraDevice.queryStringCurrentCameraDps(DpMotionSensitivity.ID);
```

#### Boolean data-point research

**Declaration**


Get the data of the corresponding function point through the cache. According to the data type of boolean, you can use the following methods to query.

```java
boolean queryBooleanCameraDps(String dpCodeID);
```

**Example**


```java
boolean dpValue = mTuyaCameraDevice.queryBooleanCameraDps(DpBasicFlip.ID);
```

### Publish data

**Declaration**


Send message through LAN or cloud

**Example**


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
mTuyaCameraDevice.publishCameraDps(DpBasicFlip.ID, true);
```


### Callback


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


## Data point id

### Basic features 

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



### Memory card management

|  Data-point  |   Data type | value                                                        |  Description                                            |              Feature definition                     |
| ------------------- | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| DpSDStatus.ID       | value                 |  1: normal, 2: abnormal (SD card is broken or under wrong format), 3: insufficient space, 4: formatting, 5: no SD card inserted |  Memory card status                              | Device reports current SD card status to the cloud, including SD card status: normal /abnormal /insufficient space etc. |
| DpSDStorage.ID      | string                | string                                                       |   Memory card capacity info research     |  Report memory card current capacity status/ capacity used/ capacity left |
| DpSDFormat.ID       | boolean               | boolean                                                      |       Memory card formatting                    | Initial formatting instruction, report current memory card formatting process |
| DpSDFormatStatus.ID | value                 |              Positive number of formatting process 0-100 |   current memory card formatting process |      Positive number of formatting process: 100 means formatting success.  Error: -2000: SD card is formatting, -2001: SD card formatting |
| DpSDRecordSwitch.ID | boolean               | True: local video recording turned on        False: local video recording turned off |  local video recording switch                  | Trun on/off SD card record: device will not record video and save to SD card after turning off the switch. |
| DpRecordMode.ID     | enum                  |                                1: event record (start to record once movement is detected)                             2: continuous record |   local video recording mode select    |   select local video recording mode, support event record and continuous record. |



### PTZ control

| Data-point |  Data type | value                                                        |  Description                      |      Feature definition           |
| ----------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------- | ------------------------------------------------------------ |
| DpPTZControl.ID   | enum                |             0: left, 2: right, 4: down, 6: up |   PTZ direction  control |                                        Use to control the PTZ, and adjust direction |
| DpPTZStop.ID      | boolean             |         bool(PTZ stop moving, no parameters) | Stop PTZ         |   PTZ stop moving, no parameters. |



### Doorbell

|  Data-point       |  Data type | value                                                        | Description                                      |                          Feature definition       |
| ------------------------ | -------------------- | ------------------------------------------------------------ | ---------------------------------------------------- | ------------------------------------------------------------ |
| DpWirelessElectricity.ID | value                |     Device power, report 0-100 integer |    Battery and device status info |   Device reports current battery power, and reports when power changes. Device sends messages to cloud current device sleep/wake status. |
| DpWirelessLowpower.ID    | value                | value                                                        |          Low power alarm value       |  Users can set the value in App. Alarm regulation: dp145≤dp147 |
| DpWirelessBatterylock.ID | boolean              |       True: locked    False: unlocked |    Battery lock                           |                  Turn on/off battery lock control |
| DpWirelessPowermode.ID   | enum                 |   0: Battery power supply, 1: Plug power supply |      Device power supply method    |      Device reports current power supply status. Report once power supply method changes. |

### Enum data points

The value range of the function point of the string enumeration type. There are corresponding string enumeration constants defined in the SDK.

| Function             | Enum                  |
| -------------------- | --------------------- |
| DpMotionSensitivity  | MotionSensitivityMode |
| DpBasicNightvision   | NightStatusMode       |
| DpPIRSwitch          | PIRMode               |
| DpRecordMode         | RecordMode            |
| DpPTZControl         | PTZDirection          |
| DpDecibelSensitivity | SoundSensitivityMode  |

