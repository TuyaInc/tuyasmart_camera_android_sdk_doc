# Business layer Callback



## Introduction

OnP2PCameraListener: Method callbacks required by the business layer

OperationDelegateCallBack: p2p signaling operation method callback



##  OnP2PCameraListener  Main method



### Callback video YUV data

```java
void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, int nFrameRate, int nIsKeyFrame, long timestamp, long nProgress, long nDuration, Object camera);
```

**Parameter Description**

| Parameter   | Description                                             |
| ----------- | ------------------------------------------------------- |
| sessionId   | p2p session Id                                          |
| y           | Video Y data                                            |
| u           | Video U data                                            |
| v           | Video V data                                            |
| width       | Video width                                             |
| height      | Video height                                            |
| timestamp   | Time stamp                                              |
| nFrameRate  | Frame rate                                              |
| nIsKeyFrame | Whether I frame                                         |
| nProgress   | ime progress (the progress of the message center video) |
| nDuration   | Duration (message center video duration)                |
| camera      | /                                                       |



### p2p session status callback

```java
void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus)
```

**Parameter Description**

| Parameter     | Description    |
| ------------- | -------------- |
| camera        | /              |
| sessionId     | session Id     |
| sessionStatus | Session status |



## OperationDelegateCallBack Main method



### p2p  operation succeeded

```java
void onSuccess(int sessionId, int requestId, String data)
```

**Parameter Description**

| Parameter | Description   |
| --------- | ------------- |
| sessionId | session Id    |
| requestId | request Id    |
| data      | Data callback |



### p2p  operation failed

**Parameter Description**

| Parameter | Description       |
| --------- | ----------------- |
| sessionId | session Id        |
| requestId | request Id        |
| errCode   | Operation errcode |

