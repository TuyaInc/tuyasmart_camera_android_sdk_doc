# 属性和方法

属性：

| 属性                     |                                 说明 |
| :----------------------- | -----------------------------------: |
| 状态码                   |              IP Camera操作相关状态码 |
| ICameraP2P                 | IOTCamera提供相关远程IP Camera的功能 |
| TuyaMonitorView          |                         视频播放控件 |
| OnP2PCameraListener |                 业务层所需的方法回调 |

方法：

```java
//TuyaCamera对象初始化
public static ICameraP2P generateTuyaSmartCamera(int  p2pType)

//注册回调监听
public void registorOnP2PCameraListener(OnP2PCameraListener listener) 

//反注册回调监听
public void removeOnP2PCameraListener()

//初始化播放器
public void generateCameraView(T view)

//删除播放器对象
public void destroyCameraView();

//初始化操作
public void createDevice(OperationDelegateCallBack callBack,int p2pType, String... args)

//建立连线
public void connect(OperationDelegateCallBack callBack, String... args) 

//断开连线
public void disconnect(OperationDelegateCallBack callBack) 

//开启live
public void startPreview(OperationDelegateCallBack callBack)

//停止live
public void stopPreview(OperationDelegateCallBack callBack)

//􏲷􏰴截图保存到指定位置
public void snapshot(String absoluteFilePath, Context context, PLAYMODE playmode, OperationDelegateCallBack callBack)

//开始本地录像，此方法是同步方法，需要在线程中处理
public int startRecordLocalMp4(String folderPath, String fileName, Context context, OperationDelegateCallBack callBack)
    
//结束本地录像
 public int stopRecordLocalMp4(OperationDelegateCallBack callBack)

//开启语音通话
public void startAudioTalk(OperationDelegateCallBack callBack)

//关闭语音通话
public void stopAudioTalk(OperationDelegateCallBack callBack)

//获取清晰度值
public void getVideoClarity(OperationDelegateCallBack callBack)

//设置清晰度
public void setVideoClarity(int mode, OperationDelegateCallBack callBack)

//设备静音功能操作
public void setMute(PLAYMODE playModel, int mute, OperationDelegateCallBack callBack)

//开始播放录像文件
public void startPlayback(int startTime, int stopTime,
                       int playTime, OperationDelegateCallBack callBack, OperationDelegateCallBack finishcallBack)

//停止播放录像文件
public void stopplayback(OperationDelegateCallBack callBack)

//暂停播放录像文件
public void pausePlayback(OperationDelegateCallBack callBack)

//继续播放录像文件
public void resumePlayback(OperationDelegateCallBack callBack)

//按月查询回放录像的存储的日期数据
public void queryRecordDaysByMonth(int year, int month, OperationDelegateCallBack callBack)

//按天查询回放录像的存储的时间片段数据
public void queryRecordTimeSliceByDay(int year, int month, int day, OperationDelegateCallBack callBack)

//销毁
public void destroyP2P();

```

