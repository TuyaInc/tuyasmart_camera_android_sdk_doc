# Business layer Callback

Callback: OnP2PCameraListener

- Method callbacks required by the business layer

method：

```java

//Callback for YUV video data
void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera)

//P2P link state callback, session Id < 0, that is, P2P disconnected
void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus)

```


Callback: OperationDelegateCallBack

function：

```java
	/**
	  * The P2P instructions are successfully operated
	  * data ：response data
	  * /
	void onSuccess(int sessionId, int requestId, String data)
	
	/**
	  * P2P instruction operation failed
	  * errCode operation error code
	  * /
	void onFailure(int sessionId, int requestId, int errCode)
```