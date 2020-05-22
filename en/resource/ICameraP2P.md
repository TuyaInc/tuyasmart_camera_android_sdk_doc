# ICameraP2P



## Introduction

ICameraP2P is the core functional interface of **Tuya Smart Camera Android SDK**, which mainly provides audio and video communication and instruction set interaction interface.



## Method



### TuyaCamera initialization

**description**

create ICameraP2P object 

```java
public static ICameraP2P generateTuyaSmartCamera(int  p2pType)
```

**Parameter Description**

| Parameter | Description                                                  |
| --------- | ------------------------------------------------------------ |
| p2pType   | corresponding to the p2p capability of the corresponding device |



### Register  listener

```java
public void registorOnP2PCameraListener(OnP2PCameraListener listener) 
```

**Parameter Description**

| Parameter           | Description            |
| ------------------- | ---------------------- |
| OnP2PCameraListener | p2p operation callback |

> OnP2PCameraListener: please refer to [callback](./Callback.md)



### Unregister  listener

```java
public void removeOnP2PCameraListener()
```



### Generate Camera View

**description**

Bind monitor view with ICameraP2P 

```java
public void generateCameraView(T view)
```

**Parameter Description**

| Parameter | Description  |
| --------- | ------------ |
| View      | Monitor View |



### Destroy Camera View

```java
public void destroyCameraView();
```



### Create Device

**description**

Associate with the underlying library p2p object device

```java
public void createDevice(OperationDelegateCallBack callback, ConfigCameraBean config)
```

**Parameter Description**

| Parameter                 | Description             |
| ------------------------- | ----------------------- |
| OperationDelegateCallBack | Operation callback      |
| ConfigCameraBean          | Configuration parameter |

> OperationDelegateCallBack: please refer to [Callback](./Callback.md)

> ConfigCameraBean: configuration parameters bean，it will return when [requestCameraInfo](./TuyaSmartCameraP2P.md), the parameters are as follows:

**Parameter Description**

| Parameter     | Description            |
| ------------- | ---------------------- |
| p2pType       | P2p Type               |
| initString    | Key configuration data |
| password      | p2p password           |
| devId         | device Id              |
| localId       | device local Id        |
| localKey      | device local Key       |
| token         | user token             |
| clientTraceId | connect traceId        |
| p2pId         | p2p Id                 |



### establish connection

**description**

build p2p connect

```java
public void connect(OperationDelegateCallBack callback);
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Disconnect

**description**

disconnect  p2p

```java
public void disconnect(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Start live

**description**

start live video 

```java
public void startPreview(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Stop live

**description**

Stop live video 

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Snapshot

**description**

Stop live video 

```java
public void snapshot(String absoluteFilePath, Context context, PLAYMODE playmode, OperationDelegateCallBack callBack)
```

**Parameter Description**

| Parameter                 | Description          |
| ------------------------- | -------------------- |
| absoluteFilePath          | Picture address path |
| context                   | /                    |
| playmode                  | Play mode            |
| OperationDelegateCallBack | Operation callback   |

> playmodel: LIVE / PLAYBACK



### Start Record Local Mp4

```java
public int startRecordLocalMp4(String folderPath, String fileName, Context context, OperationDelegateCallBack callBack)
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| folderPath                | dir path           |
| fileName                  | file name          |
| Context                   | /                  |
| OperationDelegateCallBack | Operation callback |



### Stop Record Local MP4

```java
public int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Start Audio Talk

```java
public void startAudioTalk(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Stop Audio Talk

```java
public void stopAudioTalk(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Get Video Clarity

```java
public void getVideoClarity(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Set Video Clarity

```java
public void setVideoClarity(int mode, OperationDelegateCallBack callBack)
```

**Parameter Description**

| Parameter                 | Description                      |
| ------------------------- | -------------------------------- |
| mode                      | clarity parameter: HD / STANDEND |
| OperationDelegateCallBack | Operation callback               |



### Device Mute Function Operation

```java
public void setMute(PLAYMODE playModel, int mute, OperationDelegateCallBack callBack)
```

**Parameter Description**

| Parameter                 | Description                 |
| ------------------------- | --------------------------- |
| playModel                 | Play mode                   |
| Mute                      | Mute parameter：UNMUTE/MUTE |
| OperationDelegateCallBack | Operation callback          |



### Start Play back 

```java
public void startPlayback(int startTime, int stopTime,int playTime, OperationDelegateCallBack callBack, OperationDelegateCallBack finishcallBack)
```

**Parameter Description**

| Parameter      | Description            |
| -------------- | ---------------------- |
| startTime      | timePiece start Time   |
| stopTime       | timePiece stop Time    |
| playTime       | play time              |
| callBack       | start playback success |
| finishcallBack | finish playback        |

> playTime must be between startTime and stopTime, otherwise playback will fail.



### Stop Play back

```java
public void stopplayback(OperationDelegateCallBack callBack)
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Pause Play back

```java
public void pausePlayback(OperationDelegateCallBack callBack)
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Resume Play back

```java
public void resumePlayback(OperationDelegateCallBack callBack)
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | Operation callback |



### Get the dates that have playback video recording info

```java
public void queryRecordDaysByMonth(int year, int month, OperationDelegateCallBack callBack)
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| year                      | Ex: 2020           |
| month                     | Ex: 03             |
| OperationDelegateCallBack | Operation callback |



### Get the video playback info of someday

```java
public void queryRecordTimeSliceByDay(int year, int month, int day, OperationDelegateCallBack callBack)
```

**Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| year                      | Ex: 2020           |
| month                     | Ex: 03             |
| day                       | Ex: 05             |
| OperationDelegateCallBack | Operation callback |



### Destroy

```java
public void destroyP2P();
```

