# 业务层回调

## 介绍

OnP2PCameraListener: 实现业务层所需的回调
OperationDelegateCallBack: p2p信令操作方法回调



## OnP2PCameraListener主要方法



### 回调视频YUV数据

```java
  void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera)
```
#### 参数说明
| 参数          | 说明    |
| ------------- | ------- |
| sessionId       | p2p session Id |
| y    | 视频Y数据       |
| u         | 视频U数据       |
| v      | 视频V数据 |
| width         | 视频宽度  |
| height       | 视频高度      |
| timestamp      | 时间戳     |
| camera         | /       |



### p2p的链接状态回调

```java
  void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus)
```

#### 参数说明

| 参数          | 说明         |
| ------------- | ------------ |
| camera        | /            |
| sessionId     | session Id   |
| sessionStatus | session 状态 |



## OperationDelegateCallBack主要方法：



### p2p指令操作成功

```java
void onSuccess(int sessionId, int requestId, String data)
```

#### 参数说明

| 参数      | 说明             |
| --------- | ---------------- |
| sessionId | /                |
| requestId | /                |
| data      | 可携带回来的数据 |

### p2p指令操作失败

```java
void onFailure(int sessionId, int requestId, int errCode)
```

#### 参数说明

| 参数      | 说明       |
| --------- | ---------- |
| sessionId | /          |
| requestId | /          |
| errCode   | 操作错误码 |

