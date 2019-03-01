# 属性和方法

属性：

| 属性                     |                                 说明 |
| :----------------------- | -----------------------------------: |
| 状态码                   |              IP Camera操作相关状态码 |
| MyCamera                 | IOTCamera提供相关远程IP Camera的功能 |
| TuyaMonitorView          |                         视频播放控件 |
| isRunSoft                |                         是否启动软解 |
| OnDelegateCameraListener |                 业务层所需的方法回调 |

方法：

```java
//TuyaCamera对象初始化
public static ITuyaSmartCamera generateTuyaSmartCamera(int  p2pType)

//注册回调监听
public void registorOnDelegateCameraListener(OnDelegateCameraListener listener) 

//反注册回调监听
public void unRegistorOnDelegateCameraListener()

//初始化播放器
public void generateCameraView(Object view)

//删除播放器对象
public void destroyCameraView();

//初始化操作 (p2ptype为2暂时不支持硬解)
public void createDevice(String did, String initString, boolean isRunSoft)

//建立连线
public void connect(String p2pID, String pwd,String localKey) 

//断开连线
public void disconnect() 

//构建另一条channel通道，如果要开启对讲功能，首先要调用该方法
public void connectPlayback() 

//开启live
public void startPreview()

//停止live
public void stopPreview()

//􏲷􏰴截图保存到指定位置
public void snapshot(String absoluteFilePath, Context context, ICameraP2P.PLAYMODE playmode)

//开始本地录像，此方法是同步方法，需要在线程中处理
public int startRecordLocalMp4(String folderPath, String fileName, Context context)

//不带音频的本地录像，此方法是同步方法，需要在线程中处理
public int startRecordingWithoutAudio(final String filePath, final Context context)
    
//结束本地录像
 public int stopRecordLocalMp4()

//开启语音通话
public void startAudioTalk()

//关闭语音通话
public void stopAudioTalk()

//获取清晰度值
public void getVideoClarity()

//设置清晰度
public void setVideoClarity(int mode)

//设备静音功能操作
public void setMute(ICameraP2P.PLAYMODE playModel, int mute, Context context)

//开始播放录像文件
public void startPlayback(int startTime, int stopTime, int playTime)

//停止播放录像文件
public void stopplayback()

//暂停播放录像文件
public void pausePlayback()

//继续播放录像文件
public void resumePlayback()

//按月查询回放录像的存储的日期数据
public void queryRecordDaysByMonth(int year, int month)

//按天查询回放录像的存储的时间片段数据
public void queryRecordTimeSliceByDay(int year, int month, int day)

//销毁
public void onDestroy();

```

