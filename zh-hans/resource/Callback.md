# 业务层回调

Callback: OnDelegateCameraListener

- 实现业务层所需的回调

方法：

```java
//初始化成功
void onCreateDeviceSuccess();

//初始化失败
void onCreateDeviceFail(int ret);

//connect 连线失败
public void connectFail(String errorCode, String errorMsg)

//connect live channel 连线成功
public void onChannel0StartSuccess()

//connect playback channel 连线成功 (p2ptype = 2， 不需要使用)
public void onChannel1StartSuccess()

//其他connect状态
public void onChannelOtherStatus(int errorCode)

//live播放调用成功
void onPreviewSuccess()

//live播放调用失败
void onPreviewFail(int errorCode)

//截屏操作成功
void onSnapshotSuccessCallback()
//截屏操作失败
void onSnapshotFailCallback()

//声音控制回调成功
void onMuteOperateSuccess(ICameraP2P.PLAYMODE playmode，int isMute)
//声音控制回调失败
void onMuteOperateFail(ICameraP2P.PLAYMODE playmode)

//connect playback 连线失败 (p2ptype = 2， 不需要使用)
public void onPlaybackEnterFail(String errorCode, String errorMsg)

//回放录像播放成功
public void onPlaybackStartSuccess()

//回放录像播放失败
public void onPlaybackStartFail(String errorCode, String errorMsg)

//回放录像暂停成功
public void onPlaybackPauseSuccess()

//回放录像暂停失败
void onPlaybackPauseFail(String errorCode, String errorMsg)

//回放录像继续播放成功
public void onPlaybackResumeSuccess();

//回放录像继续播放失败
public void onPlaybackResumeFail(String errorCode, String errorMsg);

//回放录像结束
public void onPlaybackEnd();

//回放录像结束失败
public void onPlaybackEndFail();

//清晰度回调
public void onDefinitionStatusCallback(boolean isQuery, int definition)

//按月查询日期数据成功
public  void onQueryPlaybackDataSuccessByMonth(int year, int month, Object days);

//按月查询日期数据失败
public  void onQueryPlaybackDataFailureByMonth(int errorCode, String errorMsg);

//按天查询时间片段数据成功
public  void onQueryPlaybackDataSuccessByDay(String yearmonthday, Object timePieceBeanList);

//按天查询时间片段数据失败
public  void onQueryPlaybackDataFailureByDay(int errorCode, String errorMsg);

//获取Iframe （p2ptype = 2不使用）
public  void onreceiveFrameDataCallback()

//对讲操作成功
public  void onSpeakSuccessCallback()

//对讲操作失败
public  void onSpeakFailueCallback(int errorCode)

//对讲操作成功
void onStopSpeakSuccessCallback()

//对讲关闭操作失败
void onStopSpeakFailueCallback(int errorCode)

//硬解模式下，接收视频数据回调 （p2ptype =2 不使用）
public void receiveFrameDataForMediaCodec(Camera var1, int var2, byte[] var3, int var4, int var5, byte[] var6, boolean var7, int var8)

//回调视频YUV数据（仅p2ptype=2的设备使用该回调）
void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera)

//p2p的链接状态回调，sessionId < 0，即p2p断开（仅p2ptype=2的设备使用该回调）
void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus)
```