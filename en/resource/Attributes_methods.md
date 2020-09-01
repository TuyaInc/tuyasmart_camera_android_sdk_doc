# property & Methods

property：

|  property       |                      statement |
| :----------------------- | -----------------------------------: |
|  status code   |  ip camera related  operate status code |
| ICameraP2P                 |  Iot camera provide related remote ip camera  function |
| TuyaMonitorView     |   video play controls |
| OnP2PCameraListener | Method callbacks required by the business layer |

methods：

```java

//Create ICameraP2P instance 
public static ICameraP2P generateTuyaSmartCamera(int  p2pType)

//register Callback
public void registorOnP2PCameraListener(OnP2PCameraListener listener) 

//remove Callback
public void removeOnP2PCameraListener()

//init cameraView
public void generateCameraView(T view)

//delete cameraView
public void destroyCameraView();


//create Camera device
public void createDevice(OperationDelegateCallBack callBack,int p2pType, String... args)


// establishing P2P connection
public void connect(OperationDelegateCallBack callBack, String... args) 

//disconnect p2p
public void disconnect(OperationDelegateCallBack callBack) 

//  start live
public void startPreview(OperationDelegateCallBack callBack)

// stop live
public void stopPreview(OperationDelegateCallBack callBack)

// save the screenshot to the specified location
public void snapshot(String absoluteFilePath, Context context, PLAYMODE playmode, OperationDelegateCallBack callBack)

//start local video
public int startRecordLocalMp4(String folderPath, String fileName, Context context, OperationDelegateCallBack callBack)

//stop local video
 public int stopRecordLocalMp4(OperationDelegateCallBack callBack)

//start audio Talk
public void startAudioTalk(OperationDelegateCallBack callBack)

//stop audio Talk
public void stopAudioTalk(OperationDelegateCallBack callBack)
 
//Obtain clarity values
public void getVideoClarity(OperationDelegateCallBack callBack)

//Setting clarity
public void setVideoClarity(int mode, OperationDelegateCallBack callBack)

//Mute function operation
public void setMute(PLAYMODE playModel, int mute, OperationDelegateCallBack callBack)


//start playback video files of the device 
public void startPlayback(int startTime, int stopTime,
                       int playTime, OperationDelegateCallBack callBack, OperationDelegateCallBack finishcallBack)
                       

//stop playback video files of the device
public void stopplayback(OperationDelegateCallBack callBack)

//pause playback video
public void pausePlayback(OperationDelegateCallBack callBack)

//resume playback video
public void resumePlayback(OperationDelegateCallBack callBack)

//query the playback video data by month
public void queryRecordDaysByMonth(int year, int month, OperationDelegateCallBack callBack)

//query the playback video of time-slice by day
public void queryRecordTimeSliceByDay(int year, int month, int day, OperationDelegateCallBack callBack)


public void destroyP2P();

void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera)


void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus)
```

