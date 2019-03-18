
# Camera function data-points





**【Description】**

Tuya smart camera supports various function data-points. The interaction between devices and APP is realized through standardized function settings. Certain return data of the device send notifications to APP by asynchronous messaging.




### Camera device standardized features


- Device basic features setting

| Data-point | Data type | value | Description |            Feature definition |
| ------ | ------ | :----: | ------ | ------ |
| DpBasicIndicator.ID | boolean |                                         True： turn on indicator under normal state                                 False: turn off indicator under normal state | Status indicator | Turn on/off device indicator under normal state: avoid light pollution at night. Indicator needs to be turned on during wifi configuration process or device is abnormal. |
| DpBasicFlip.ID | boolean |                 True: flip               False: normal status                 | Screen flip |                          Turn on/off screen vertical flip: adjust screen direction in the case when the device is upside down. |
| DpBasicOSD.ID | boolean | True: OSD appears         False: OSD disappears              |  Time watermark |  Turn on/off time watermark in live view mode |
| DpBasicPrivate.ID | boolean | True: private mode turned on             False: private mode turned off | Private mode | Turn on/off private mode: no video&audio stream collecting, the device has no live view and playback |
| DpBasicNightvision.ID | enum | Auto, 1: Off, 2: On |  IR night vision | Select IR night vision status On/Off/Auto |

- Motion detection alarm

| Data-point     | Data type | value                                                        | Description                                              | Feature definition |
| ---------------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| DpPIRSwitch.ID         | enum                | 0：off, 1: low sensitivity, 2: medium sensitivity, 3: high sensitivity | PIR switch and sensitivity          |  Adjust PIR status: Off/low sensitivity/medium sensitivity/ high sensitivity |
| DpMotionSwitch.ID      | boolean             | True: motion detection alarm turned on               False: motion detection alarm turned off | Motion detection alarm switch               |  Turn on/off motion detection alarm. After turning on: device needs to report messages to server once movement is detected under suitable conditions. |
| DpMotionSensitivity.ID | enum                |  0: low,1:medium, 2:high             |  motion detection sensitivity setting |   Set motion detection alarm sensitivity |


- Sound detection alarm

| Data-point       |  Data type | value                                                        |      Description                                        |                                Feature definition |
| ----------------------- | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| DpDecibelSwitch.ID      | boolean               |   True: sound detection alarm turned on              False: sound detection alarm turned off |  Sound detection alarm switch               |  Turn on/off sound detection alarm. After turning on: device needs to report messages to server once sound is detected under suitable conditions. |
| DpDecibelSensitivity.ID | enum                  | low,1:medium, 2:high |  sound detection sensitivity setting | Set sound detection alarm sensitivity |


- Memory card and local video recording management

|  Data-point  |   Data type | value                                                        |  Description                                            |              Feature definition                     |
| ------------------- | --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| DpSDStatus.ID       | value                 |  1: normal, 2: abnormal (SD card is broken or under wrong format), 3: insufficient space, 4: formatting, 5: no SD card inserted |  Memory card status                              | Device reports current SD card status to the cloud, including SD card status: normal /abnormal /insufficient space etc. |
| DpSDStorage.ID      | string                | string                                                       |   Memory card capacity info research     |  Report memory card current capacity status/ capacity used/ capacity left |
| DpSDFormat.ID       | boolean               | boolean                                                      |       Memory card formatting                    | Initial formatting instruction, report current memory card formatting process |
| DpSDFormatStatus.ID | value                 |              Positive number of formatting process 0-100 |   current memory card formatting process |      Positive number of formatting process: 100 means formatting success.  Error: -2000: SD card is formatting, -2001: SD card formatting |
| DpSDRecordSwitch.ID | boolean               | True: local video recording turned on        False: local video recording turned off |  local video recording switch                  | Trun on/off SD card record: device will not record video and save to SD card after turning off the switch. |
| DpRecordMode.ID     | enum                  |                                1: event record (start to record once movement is detected)                             2: continuous record |   local video recording mode select    |   select local video recording mode, support event record and continuous record. |


- PTZ functions

| Data-point |  Data type | value                                                        |  Description                      |      Feature definition           |
| ----------------- | ------------------- | ------------------------------------------------------------ | ------------------------------------- | ------------------------------------------------------------ |
| DpPTZControl.ID   | enum                |             0: left, 2: right, 4: down, 6: up |   PTZ direction  control |                                        Use to control the PTZ, and adjust direction |
| DpPTZStop.ID      | boolean             |         bool(PTZ stop moving, no parameters) | Stop PTZ         |   PTZ stop moving, no parameters. |


- Battery powered product features

|  Data-point       |  Data type | value                                                        | Description                                      |                          Feature definition       |
| ------------------------ | -------------------- | ------------------------------------------------------------ | ---------------------------------------------------- | ------------------------------------------------------------ |
| DpWirelessElectricity.ID | value                |     Device power, report 0-100 integer |    Battery and device status info |   Device reports current battery power, and reports when power changes. Device sends messages to cloud current device sleep/wake status. |
| DpWirelessLowpower.ID    | value                | value                                                        |          Low power alarm value       |  Users can set the value in App. Alarm regulation: dp145≤dp147 |
| DpWirelessBatterylock.ID | boolean              |       True: locked    False: unlocked |    Battery lock                           |                  Turn on/off battery lock control |
| DpWirelessPowermode.ID   | enum                 |   0: Battery power supply, 1: Plug power supply |      Device power supply method    |      Device reports current power supply status. Report once power supply method changes. |



### Device standardized feature method call


ITuyaCameraDevice provides device info communication ability, control orders issue, reach current data-points info

```java
public interface ITuyaCameraDevice {
	// Data-point supports or not
    boolean isSupportCameraDps(String dpCode);
	//Search Object data-point info
    Object queryObjectCameraDps(String dpCode);
	//Search boolean data-point info
    boolean queryBooleanCameraDps(String dpCode);
	//Search String、enum data-point info
    String queryStringCurrentCameraDps(String dpCode);
	//Search value data-point info
    int queryIntegerCurrentCameraDps(String dpCode);
    //register data-point callback/listener
    void registorTuyaCameraDeviceControlCallback(String dpCode, ITuyaCameraDeviceControlCallback callback);
    //cancel data-point callback/listener
    void unRegistorTuyaCameraDeviceControlCallback(String dpCode);
	//low-power doorbell wake up
    void wirelessWake(String localKey, String devId);
	//data publish
    void publishCameraDps(String dpCode, Object value);
	//resource cleaning
    void onDestroy();
}
```



#### Obtain objective

```java
//initaling device by device ID
ITuyaCameraDevice tuyaCameraDevice = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
```



#### Callback


 ITuyaCameraDeviceControlCallback provides listener device info data receiving, and receive device callback  after data publishing from App side. Callback is able to set as Boolean/String/Integer when the data-point type is Boolean/Enum/Value.

```java
public interface ITuyaCameraDeviceControlCallback<E> {
    //Callback succeed
    void onSuccess(String devId, ACTION action, SUB_ACTION subAction, E o);
	//Callback fail
    void onFailure(String devId, ACTION action, SUB_ACTION subAction, String errorCode, String errorString);
}
```

**【usage】**

```java
//The data-point needs to register ITuyaCameraDeviceControlCallback in order to receive certain data callback
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpSDStatus.ID, new ITuyaCameraDeviceControlCallback<Integer>() {
                    @Override
                    public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, Integer o) {
                        showPublishTxt.setText("LAN/Cloud query result: " + o);
                    }

                    @Override
                    public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

                    }
                });
```




#### Data-point support or not


**【Description】**


Judge whether the device supports the data-point. Call data issues/data research will fail if the device dose not support certain data-point. 

```java
boolean isSupportCameraDps(String dpCodeID);
```



【Method call】

```java
boolean isSupportDpBasicFlip = mTuyaCameraDevice.isSupportCameraDps(DpBasicFlip.ID);
```



#### Data publish


**【Description】**

Send message through LAN or cloud



**【Method call】**

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



#### Data research


**【Description】**

Obtain data-point info through cache



```java
    //	Support enum、value、boolean、String
	Object queryObjectCameraDps(String dpCodeID);
	//Support Boolean only
    boolean queryBooleanCameraDps(String dpCodeID);
	//Support enum\string only
    String queryStringCurrentCameraDps(String dpCodeID);
	//Support value only
    int queryIntegerCurrentCameraDps(String dpCodeID);
```

>  note: queryObjectCamera Dps for queries, Developers need separate data types.







**【Method call】**


Value data-point research

```java
int dpvalue = mTuyaCameraDevice.queryIntegerCurrentCameraDps(DpSDStatus.ID);
```


Support all data-point research

```java
Object dpValue = mTuyaCameraDevice.queryObjectCameraDps(DpBasicFlip.ID);
```


Support String/Enum data-point research

```java
String mode = mTuyaCameraDevice.queryStringCurrentCameraDps(DpMotionSensitivity.ID);
```


Support boolean data-point research

```java
boolean dpValue = mTuyaCameraDevice.queryBooleanCameraDps(DpBasicFlip.ID);
```




### Others


Motion detection alarm sensitivity enum

```java
public enum MotionSensitivityMode {
    HIGH("2"),
    MIDDLE("1"),
    LOW("0");

    private String dpValue;

    private MotionSensitivityMode(String dpValue) {
        this.dpValue = dpValue;
    }

    public String getDpValue() {
        return this.dpValue;
    }
}
```


How to use:

```java
mTuyaCameraDevice.publishCameraDps(DpMotionSensitivity.ID, MotionSensitivityMode.HIGH.getDpValue(), null, new ITuyaCameraDeviceControlCallback<String>() {
                    @Override
                    public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String o) {
                        showPublishTxt.setText("LAN/Cloud query result: " + o);
                    }

                    @Override
                    public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

                    }
                });
```


IR night vision

```java
public enum NightStatusMode {
    AUTO("0"),CLOSE("1"),OPEN("2");

    private String dpValue;

    NightStatusMode(String dpValue){
        this.dpValue = dpValue;
    }

    public String getDpValue() {
        return dpValue;
    }
}
```


PIR sensitivity

```java
public enum PIRMode {
    CLOSE("0"),LOW("1"),MID("2"),HIGH("3");

    private String dpValue;

    PIRMode(String dpValue) {
        this.dpValue = dpValue;
    }

    public String getDpValue() {
        return dpValue;
    }
}
```


PTZ control

```java

public enum PTZDirection {
    UP("0"),RIGHT("2"),
    DOWN("4"),LEFT("6");

    private String dpValue;

    PTZDirection(String dpValue){
        this.dpValue = dpValue;
    }

    public String getDpValue() {
        return dpValue;
    }
}

```


Record method select

```java

public enum RecordMode {
    EVENT("1"),CONTINUOUS_RECORD("2");

    private String dpValue;

    RecordMode(String dpValue) {
        this.dpValue = dpValue;
    }

    public String getDpValue() {
        return dpValue;
    }
}
```


Sound detection sensitivity

```java
public enum SoundSensitivityMode {
    LOW("0"),HIGH("1");

    private String dpValue;

    SoundSensitivityMode(String dpValue) {
        this.dpValue = dpValue;
    }

    public String getDpValue() {
        return dpValue;
    }
}
```




#### Memory card and local video recording management


sdcard LAN/Cloud data publish dose not need other parameters


- DpSDStatus data publish

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


- DpSDStorge data publish


  Precondition: confirm whether the data-point&sdcard are exist before data is published 

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

- DpSDFormat data publish

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

- DpSDFormatStatus data publish

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

- DpSDRecordSwitch data publish

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

- DpSDRecordModel data publish

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




#### Low-power doorbell features

- Wake-up feature data publish

```java
String mDevId = devBean.getDevId();
String mLocalkey = devBean.getLocalKey();
mTuyaCameraDevice.wirelessWake（mDevId,mLocalkey）;
```

- Battery lock data publish

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

- Batter and device status info data publish

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

- Low battery alarm value data publish

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

- Power supply method data publish

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