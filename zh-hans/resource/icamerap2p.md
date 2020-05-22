# ICameraP2P



## 描述

**ICameraP2P** 是 **Tuya Smart Camera Android SDK** 核心功能接口，主要提供了音视频通信和指令集交互接口。



## 方法




### TuyaCamera 对象初始化
```java
public static ICameraP2P generateTuyaSmartCamera(int  p2pType)
```
**参数说明**

| 参数    | 说明                                  |
| :------- | :------------------------------------- |
| p2pType | p2p 类型，对应匹配相应设备的 p2p 能力 |



### 注册回调监听

```java
public void registorOnP2PCameraListener(OnP2PCameraListener listener) 
```

**参数说明**

| 参数                | 说明         |
| :------------------- | :------------ |
| OnP2PCameraListener | p2p 操作回调 |

> OnP2PCameraListener: 具体请 [查阅](./Callback.md)



### 反注册回调监听

```java
public void removeOnP2PCameraListener()
```



### 绑定播放器

```java
public void generateCameraView(T view)
```

**参数说明**

| 参数 | 说明           |
| :---- | :-------------- |
| View | 播放器组件对象 |



### 删除播放器对象
```java
public void destroyCameraView();
```



### 创建设备

**描述**

建立关联与底层库 p2p 对象设备

```java
public void createDevice(OperationDelegateCallBack callback, ConfigCameraBean config)
```

**参数说明**

| 参数                      | 说明         |
| :------------------------- | :------------ |
| OperationDelegateCallBack | 操作回调     |
| ConfigCameraBean          | 配置参数对象 |

> OperationDelegateCallBack :为此次操作回调，请 [查阅](./Callback.md)

> ConfigCameraBean :为配置参数对象，在 [获取 IPC Config 数据](./TuyaSmartCameraP2P.md) 会返回该模型实例，参数如下:

| 参数          | 说明         |
| :------------- | :------------ |
| p2pType       | p2p 类型     |
| initString    | 关键配置数据 |
| p2pId         | p2p Id       |
| password      | p2p 秘钥     |
| devId         | 设备 id      |
| localId       | /            |
| localKey      | /            |
| token         | /            |
| clientTraceId | /            |



### 建立连线

```java
public void connect(OperationDelegateCallBack callback);
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 断开连线

```java
public void disconnect(OperationDelegateCallBack callBack);
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 开启 live

```java
public void startPreview(OperationDelegateCallBack callBack);
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 停止 live

```java
public void stopPreview(OperationDelegateCallBack callBack);
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 􏲷􏰴截图保存到指定位置

```java
public void snapshot(String absoluteFilePath, Context context, PLAYMODE playmode, OperationDelegateCallBack callBack)
```

**参数说明**

| 参数                      | 说明         |
| :------------------------- | :------------ |
| absoluteFilePath          | 图片地址路径 |
| context                   | 上下文       |
| playmode                  | 播放模式     |
| OperationDelegateCallBack | 操作回调     |

> playmodel: 分为 LIVE，PLAYBACK



### 开始本地录像
```java
public int startRecordLocalMp4(String folderPath, String fileName, Context context, OperationDelegateCallBack callBack)
```

**参数说明**

| 参数                      | 说明               |
| :------------------------- | :------------------ |
| folderPath                | 视频文件夹地址路径 |
| fileName                  | 上下文             |
| Context                   | 播放模式           |
| OperationDelegateCallBack | 操作回调           |



### 结束本地录像

```java
public int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 开启语音通话

```java
public void startAudioTalk(OperationDelegateCallBack callBack);
```

#### 参数说明

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 关闭语音通话

```java
public void stopAudioTalk(OperationDelegateCallBack callBack);
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 获取清晰度值

```java
public void getVideoClarity(OperationDelegateCallBack callBack);
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 设置清晰度

```java
public void setVideoClarity(int mode, OperationDelegateCallBack callBack)
```

**参数说明**

| 参数                      | 说明                      |
| :------------------------- | :------------------------- |
| mode                      | 清晰度参数：HD / STANDEND |
| OperationDelegateCallBack | 操作回调                  |



### 设备静音功能操作

```java
public void setMute(PLAYMODE playModel, int mute, OperationDelegateCallBack callBack)
```

**参数说明**

| 参数                      | 说明                  |
| :------------------------- | :--------------------- |
| playModel                 | 播放模式              |
| Mute                      | 静音参数：UNMUTE/MUTE |
| OperationDelegateCallBack | 操作回调              |



### 开启播放回放录像文件

```java
public void startPlayback(int startTime, int stopTime,int playTime, OperationDelegateCallBack callBack, OperationDelegateCallBack finishcallBack)
```

**参数说明**

| 参数                      | 说明                 |
| :------------------------- | :-------------------- |
| startTime                 | 片段开始时间         |
| stopTime                  | 片段结束时间         |
| playTime                  | 播放时间             |
| callBack                  | 开启播放回放操作回调 |
| OperationDelegateCallBack | 播放结束操作回调     |

> playTime 必须在 startTime、stopTime 之间，否则会播放失败



### 停止播放录像文件

```java
public void stopplayback(OperationDelegateCallBack callBack)
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 暂停播放录像文件

```java
public void pausePlayback(OperationDelegateCallBack callBack)
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 继续播放录像文件

```java
public void resumePlayback(OperationDelegateCallBack callBack)
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| OperationDelegateCallBack | 操作回调 |



### 按月查询回放录像的存储的日期数据

```java
public void queryRecordDaysByMonth(int year, int month, OperationDelegateCallBack callBack)
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| year                      | 年       |
| month                     | 月       |
| OperationDelegateCallBack | 操作回调 |



### 按天查询回放录像的存储的时间片段数据

```java
public void queryRecordTimeSliceByDay(int year, int month, int day, OperationDelegateCallBack callBack)
```

**参数说明**

| 参数                      | 说明     |
| :------------------------- | :-------- |
| year                      | 年       |
| month                     | 月       |
| day                       | 日       |
| OperationDelegateCallBack | 操作回调 |



### 销毁

```java
public void destroyP2P();
```



 



