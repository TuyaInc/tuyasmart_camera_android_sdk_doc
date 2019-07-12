# 业务层回调

Callback: OnP2PCameraListener

- 实现业务层所需的回调

方法：

```java

//回调视频YUV数据
void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera)

//p2p的链接状态回调，sessionId < 0，即p2p断开（仅p2ptype=2的设备使用该回调）
void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus)

```

Callback: OperationDelegateCallBack

方法：

```java
	//p2p指令操作成功，sessionId, requestId目前没有使用。
	//data表示可携带回来的数据
	void onSuccess(int sessionId, int requestId, String data)
	//p2p指令操作失败，sessionId, requestId目前没有使用。
	//errCode 表示操作错误码
	void onFailure(int sessionId, int requestId, int errCode)
```
