# 业务层回调



## 简介

- OnP2PCameraListener：实现业务层所需的回调
- OperationDelegateCallBack：p2p 信令操作方法回调





## OnP2PCameraListener 主要方法



### 回调视频 YUV 数据

```java
void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, int nFrameRate, int nIsKeyFrame, long timestamp, long nProgress, long nDuration, Object camera);
```
**参数说明**

| 参数        | 说明                             |
| :----------- | :-------------------------------- |
| sessionId   | /                                |
| Y           | 视频Y数据                        |
| u           | 视频U数据                        |
| v           | 视频V数据                        |
| width       | 视频画面的宽                     |
| height      | 视频画面的高                     |
| nFrameRate  | 帧率                             |
| nIsKeyFrame | 是否I帧                          |
| timestamp   | 时间戳                           |
| nProgress   | 时间进度(消息中心视频播放的进度) |
| nDuration   | 时长(消息中心视频播放时长)       |
| camera      | /                                |

### p2p 的链接状态回调

```java
  void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus)
```

#### 参数说明

| 参数          | 说明         |
| :------------- | :------------ |
| camera        | /            |
| sessionId     | session Id   |
| sessionStatus | session 状态 |



## OperationDelegateCallBack 主要方法：



### p2p 指令操作成功

```java
void onSuccess(int sessionId, int requestId, String data)
```

**参数说明**

| 参数      | 说明             |
| :--------- | :---------------- |
| sessionId | /                |
| requestId | /                |
| data      | 可携带回来的数据 |



### p2p 指令操作失败

```java
void onFailure(int sessionId, int requestId, int errCode)
```

**参数说明**

| 参数      | 说明       |
| :--------- | :---------- |
| sessionId | /          |
| requestId | /          |
| errCode   | 操作错误码 |

