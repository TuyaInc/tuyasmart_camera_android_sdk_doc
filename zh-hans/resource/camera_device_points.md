# 摄像头设备功能点

涂鸦智能摄像头设备提供了丰富的功能点，并且通过标准化来实现设备与App之间的交互。设备相关的返回数据都采用异步消息方式通知APP。



## 摄像头设备标准化功能



### 设备基本设置功能

| 功能点 | 数据类型 | value | 描述 | 功能定义 |
| ------ | ------ | :----: | ------ | ------ |
| DpBasicIndicator.ID | boolean | true为正常状态下开启指示灯，false为正常状态下关闭指示灯 | 状态指示灯 | 开启/关闭设备正常状态下的指示灯，在晚间避免光污染。配网阶段、设备故障等情况下指示灯仍需要正常显示以提示用户 |
| DpBasicFlip.ID | boolean |                 true为翻转，false为正常                 | 画面翻转 | 开启/关闭画面垂直翻转，用于设备倒装时调整画面方向 |
| DpBasicOSD.ID | boolean |              true为打开OSD，false为关闭OSD              | 时间水印 | 开启/关闭直播画面的时间水印 |
| DpBasicPrivate.ID | boolean | true为开启隐私模式，false为关闭隐私模式 | 隐私模式 | 开启/关闭隐私模式，休眠时设备的音视频采集不工作，即设备无法直播也无法保存录像 |
| DpBasicNightvision.ID | enum | 0：自动, 1：关, 2：开 | 红外夜视功能 | 调整红外夜视功能的开/关/自动状态 |

### 移动侦测报警

| 功能点                 | 数据类型 | value                                           | 描述                   | 功能定义                                                     |
| ---------------------- | -------- | ----------------------------------------------- | ---------------------- | ------------------------------------------------------------ |
| DpPIRSwitch.ID         | enum     | 0：关闭，1：低灵敏度，2：中灵敏度，3：高灵敏度  | PIR开关及灵敏度        | 调整PIR功能的关闭/低灵敏度/中灵敏度/高灵敏度                 |
| DpMotionSwitch.ID      | boolean  | true为开启移动侦测报警，false为关闭移动侦测报警 | 移动侦测报警开关       | 开启/关闭移动侦测报警，开启后，设备需要在适当的条件下每次检测到移动均上报消息到服务器 |
| DpMotionSensitivity.ID | enum     | 0：低,1：中, 2：高                              | 移动侦测报警灵敏度设置 | 设置移动侦测报警的灵敏度                                     |

### 声音侦测报警

| 功能点                  | 数据类 型 | value                                           | 描述                   | 功能定义                                                     |
| ----------------------- | --------- | ----------------------------------------------- | ---------------------- | ------------------------------------------------------------ |
| DpDecibelSwitch.ID      | boolean   | true为打开声音侦测报警，false为关闭声音侦测报警 | 声音侦测报警开关       | 开启/关闭声音侦测报警，开启后，设备需要在适当的条件下每次检测到声音均上报消息到服务器 |
| DpDecibelSensitivity.ID | enum      | 0：低灵敏度,1：高灵敏度                         | 声音侦测报警灵敏度设置 | 设置声音侦测报警的灵敏度                                     |

### 存储卡及本地录像管理

| 功能点              | 数据类型 | value                                                        | 描述                         | 功能定义                                                     |
| ------------------- | -------- | ------------------------------------------------------------ | ---------------------------- | ------------------------------------------------------------ |
| DpSDStatus.ID       | value    | 1：正常,2：异常（SD卡损坏或格式不对）,3：空间不足, 4：正在格式化,5：无SD卡 | 存储卡状态                   | 设备向云端上报当前SD卡的状态，包括SD卡正常、异常、空间不足等 |
| DpSDStorage.ID      | string   | string                                                       | 存储卡容量信息查询           | 上报存储卡当前的容量状态，包括总容量、已使用容量、剩余容量   |
| DpSDFormat.ID       | boolean  | boolean                                                      | 存储卡格式化                 | 发起格式化存储卡的指令、上报当前存储卡格式化的进度等状态     |
| DpSDFormatStatus.ID | value    | 正数为格式化进度0-100                                        | 当前存储卡格式化的进度等状态 | 正数为格式化进度，100即格式化成功.错误码：-2000：SD卡已在格式化状态，-2001：SD卡格式化异常 |
| DpSDRecordSwitch.ID | boolean  | true为打开本地录像，false为关闭本地录像                      | 本地录像开关                 | 开启/关闭SD卡录像，关闭后设备不会将实时录像记录在SD卡        |
| DpRecordMode.ID     | enum     | 1：事件录像（检测到移动再录像到SD卡）2：连续录像             | 本地录像模式选择             | 在本地录像开启的状态下，选择录像的模式，支持“事件录像”和“连续录像” |

### PTZ(云台方向控制)功能

| 功能点          | 数据类型 | value                          | 描述             | 功能定义                             |
| --------------- | -------- | ------------------------------ | ---------------- | ------------------------------------ |
| DpPTZControl.ID | enum     | 0：上,2：右,4：下,6：左        | 云台方向控制     | 用于控制云台开始转动以及转动的方向。 |
| DpPTZStop.ID    | boolean  | bool(云台停止转动命令，无参数) | 控制云台停止转动 | 云台停止转动命令，无参数             |

### 电池供电产品功能

| 功能点                   | 数据类型 | value                     | 描述               | 功能定义                                                     |
| ------------------------ | -------- | ------------------------- | ------------------ | ------------------------------------------------------------ |
| DpWirelessElectricity.ID | value    | 设备电量，上报0-100的整数 | 电池及设备状态信息 | 设备主动上报当前电量，电量发生变化时上报，同时设备及时通知云端当前休眠/唤醒状态 |
| DpWirelessLowpower.ID    | value    | value                     | 低电量报警阈值     | 用户可以在App上设置阈值，报警规则为dp145≤dp147则报警         |
| DpWirelessBatterylock.ID | boolean  | true为上锁，false为解锁   | 电池锁             | 控制打开/关闭电池锁                                          |
| DpWirelessPowermode.ID   | enum     | 0：电池供电,1：插电供电   | 设备供电方式查看   | 设备主动上报当前供电状态，供电状态发生变化时上报             |





## 设备标准化功能方法调用

ITuyaCameraDevice提供与设备信息通信的能力，提供了控制指令下发、获取当前功能点的数据。



### 获取对象

**描述**

根据设备id初始化设备控制类

```java
ITuyaCameraDevice getCameraDeviceInstance(String devId) 
```



**示例代码**

```java
ITuyaCameraDevice tuyaCameraDevice = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
```



#### 功能点是否支持

**描述**

通过该方法可以判断设备是否支持该功能点，如果设备不支持该功能，调用**数据下发**和**数据查询**的方法是会失败的，因此做数据下发和查询之前要先判断该功能是否支持。**入参**为功能点的ID

```java
boolean isSupportCameraDps(String dpCodeID);
```

**示例代码**

```java
boolean isSupportDpBasicFlip = mTuyaCameraDevice.isSupportCameraDps(DpBasicFlip.ID);
```



#### 数据下发

**描述**

通过局域网或者云端这两种方式发送指令

```java
void publishCameraDps(String dpCode, Object value);
```



**示例代码**

```java
//如果功能点的返回值是Boolean，回调可以设置Boolean；如果是enum/String，回调设置String；如果是value，回调设置Integer
mTuyaCameraDevice.registorTuyaCameraDeviceControlCallback(DpBasicFlip.ID, new ITuyaCameraDeviceControlCallback<Boolean>() {
                    @Override
                    public void onSuccess(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, Boolean o) {
                        showPublishTxt.setText("LAN/Cloud query result: " + o);
                    }

                    @Override
                    public void onFailure(String s, DpNotifyModel.ACTION action, DpNotifyModel.SUB_ACTION sub_action, String s1, String s2) {

                    }
                });
mTuyaCameraDevice.publishCameraDps(DpBasicFlip.ID, true);
```

####  ITuyaCameraDeviceControlCallback 回调

 ITuyaCameraDeviceControlCallback类提供监听设备信息数据接收，以及app端数据下发后，收到设备回传来的回调。如果功能点的数据类型是Boolean，回调可以设置Boolean；如果是枚举，回调设置String；如果是value，回调设置Integer。

```java
public interface ITuyaCameraDeviceControlCallback<E> {
    //成功回调
    void onSuccess(String devId, ACTION action, SUB_ACTION subAction, E o);
	  //失败回调
    void onFailure(String devId, ACTION action, SUB_ACTION subAction, String errorCode, String errorString);
}
```

支持的数据类型：

| 数据类型 | 描述           |
| -------- | -------------- |
| Boolean  | 布尔型         |
| String   | 字符型、枚举型 |
| Value    | 数值型         |



```java
public interface ITuyaCameraDevice {
	//功能点是否支持
    
	//查询Object的功能点数据
    Object queryObjectCameraDps(String dpCode);
	//查询boolean的功能点数据
    boolean queryBooleanCameraDps(String dpCode);
	//查询String、enum的功能点数据
    String queryStringCurrentCameraDps(String dpCode);
	//查询value的功能点数据
    int queryIntegerCurrentCameraDps(String dpCode);
    //注册功能点回调监听
    void registorTuyaCameraDeviceControlCallback(String dpCode, ITuyaCameraDeviceControlCallback callback);
    //取消功能点回调监听
    void unRegistorTuyaCameraDeviceControlCallback(String dpCode);
	//低功耗门铃唤醒
    void wirelessWake(String localKey, String devId);
	
	//清除资源
    void onDestroy();
}
```



#### Value数据查询

**【描述】**

通过缓存获取对应功能点的数据，根据Value的数据类型可以分别用以下方法查询

```java
 int queryIntegerCurrentCameraDps(String dpCodeID);
```

**示例代码**

```java
int dpvalue = mTuyaCameraDevice.queryIntegerCurrentCameraDps(DpSDStatus.ID);
```



#### Object数据查询

**【描述】**

通过缓存获取对应功能点的数据，支持enum、value、boolean、String所有的功能点查询

```java
Object queryObjectCameraDps(String dpCodeID);
```

**示例代码**

```java
Object dpValue = mTuyaCameraDevice.queryObjectCameraDps(DpBasicFlip.ID);
```

> 如果使用：queryObjectCameraDps进行查询，需要开发者对数据类型进行单独区分

```java
 
	//只支持boolean
    boolean queryBooleanCameraDps(String dpCodeID);
	//只支持enum、String	
    String queryStringCurrentCameraDps(String dpCodeID);
	//只支持value
   
```



#### String/Enum数据查询

**【描述】**

通过缓存获取对应功能点的数据，根据String/Enum的数据类型可以分别用以下方法查询

```java
 String queryStringCurrentCameraDps(String dpCodeID);
```

**示例代码**

```java
String mode = mTuyaCameraDevice.queryStringCurrentCameraDps(DpMotionSensitivity.ID);
```



#### Boolean数据查询

**【描述】**

通过缓存获取对应功能点的数据，根据boolean的数据类型可以分别用以下方法查询

```java
 boolean queryBooleanCameraDps(String dpCodeID);
```

**示例代码**

```java
boolean dpValue = mTuyaCameraDevice.queryBooleanCameraDps(DpBasicFlip.ID);
```



### 其他类

#### 移动侦测报警的灵敏度的枚举类

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

**示例代码**

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

#### 红外夜视的枚举类

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

#### PIR灵敏度的枚举类

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

#### 云台控制的枚举类

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

#### 录像模式的枚举类

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

#### 声音灵敏度的枚举类

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



### 存储卡及本地录像管理

**描述**

sdcard的LAN/Cloud数据下发是不需要传其他参数的。



#### DpSDStatus的数据下发

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

#### DpSDStorge的数据下发

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

#### DpSDFormat的数据下发

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

#### DpSDFormatStatus的数据下发

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

#### DpSDRecordSwitch的数据下发

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

#### DpSDRecordModel的数据下发

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



### 低功耗门铃功能



#### 唤醒功能的数据下发

```java
void wirelessWake(String localKey, String devId);
```



**示例代码**

```java
String mDevId = devBean.getDevId();
String mLocalkey = devBean.getLocalKey();
mTuyaCameraDevice.wirelessWake（mDevId,mLocalkey）;
```



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

> 注意：DpWirelessPowermode数据下发不需要带参数，所以传null值即可。