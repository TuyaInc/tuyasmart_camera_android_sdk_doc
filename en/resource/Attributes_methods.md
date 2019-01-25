# property & Methods

property：

|  property       |                      statement |
| :----------------------- | -----------------------------------: |
|  status code   |  ip camera related  operate status code |
| MyCamera                 |  Iot camera provide related remote ip camera  function |
| TuyaMonitorView     |   video play controls |
| isRunSoft                | whether start soft decode |
| OnDelegateCameraListener | Method callbacks required by the business layer |

methods：

```java

// initialization successed
void onCreateDeviceSuccess();

// initialization failed
void onCreateDeviceFail(int ret);

// connection failed
public void connectFail(String errorCode, String errorMsg)

//live channel connection succeed
public void onChannel0StartSuccess()

// playback channel connection succeed (p2ptype = 2, no need to call)
public void onChannel1StartSuccess()

// other connection status
public void onChannelOtherStatus(int errorCode)

//  call live succeed
void onPreviewSuccess()

// call live failed
void onPreviewFail(int errorCode)

// snapshot succeed
void onSnapshotSuccessCallback()
// snapshot failed
void onSnapshotFailCallback()

// voice control call back succeed
void onMuteOperateSuccess(ICameraP2P.PLAYMODE playmode，int isMute)
// voice control call back failed
void onMuteOperateFail(ICameraP2P.PLAYMODE playmode)

// playback channel connection failed (p2ptype = 2, no need to call)
public void onPlaybackEnterFail(String errorCode, String errorMsg)

// playback succeed
public void onPlaybackStartSuccess()

// playback failed
public void onPlaybackStartFail(String errorCode, String errorMsg)

// playback pause succeed
public void onPlaybackPauseSuccess()

// playback pause failed
void onPlaybackPauseFail(String errorCode, String errorMsg)

// playback resume succeed
public void onPlaybackResumeSuccess();

// playback resume failed
public void onPlaybackResumeFail(String errorCode, String errorMsg);

// end playback 
public void onPlaybackEnd();

// end playback failed
public void onPlaybackEndFail();

// definition status call back
public void onDefinitionStatusCallback(boolean isQuery, int definition)

// query playback data by month succeed 
public  void onQueryPlaybackDataSuccessByMonth(int year, int month, Object days);

// query playback data by month failed 
public  void onQueryPlaybackDataFailureByMonth(int errorCode, String errorMsg);

// query playback fragments data by day succeed
public  void onQueryPlaybackDataSuccessByDay(String yearmonthday, Object timePieceBeanList);

// query playback fragments data by day failed
public  void onQueryPlaybackDataFailureByDay(int errorCode, String errorMsg);

// get I frame (p2ptype = 2 not use)
public  void onreceiveFrameDataCallback()

// intercom succeed
public  void onSpeakSuccessCallback()

// intercom failed
public  void onSpeakFailueCallback(int errorCode)

// stop intercom succeed
void onStopSpeakSuccessCallback()

// stop intercom failed
void onStopSpeakFailueCallback(int errorCode)

//（p2ptype =2 not use）
public void receiveFrameDataForMediaCodec(Camera var1, int var2, byte[] var3, int var4, int var5, byte[] var6, boolean var7, int var8)

//（p2ptype=1 not use）
void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera)

//（p2ptype=1 not use）
void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus)
```

