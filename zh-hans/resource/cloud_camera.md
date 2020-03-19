# 云存储功能

**【描述】**

涂鸦智能摄像头设备提供了云存储服务,因此我们也给 Android 客户端开发者提供了相应 IPC Camera 云存储 API 功能，来实现展示云端化的音视频数据。



**【引入方式】**

```java
implementation 'io.reactivex.rxjava2:rxandroid:2.0.1'
implementation 'io.reactivex.rxjava2:rxjava:2.1.7'
```



## 云存储 API 接口

ICloudCameraManager 提供云存储视频数据获取和播放相关的能力。

```java
public interface ICloudCameraManager {
/**
     * 注册播放回调
     * @param listener
     */
     void registorOnCloudCameraVideoListener(OnCloudCameraVideoListener listener);

    /**
     * 初始化
     * @param deviceBean
     * @param callback
     */
     void initCameraCloud(DeviceBean deviceBean, CloudCameraManager.ICloudManagerCallback callback);

    /**
     * 获取云存储事件
     * @param dayBean
     * @param devId
     * @param offset
     * @param limit
     * @param callback
     */
     void getTimeRange(CloudDayBean dayBean, String devId, int offset, int limit, CloudCameraManager.ICloudManagerCallback callback);

    /**
     * 获取云存储事件片段
     * @param devId
     * @param timeGT
     * @param timeLT
     * @param callback
     */
     void getCloudTimeLine(final String devId, final String timeGT, final String timeLT, CloudCameraManager.ICloudManagerCallback callback);

    /**
     * 播放云存储视频
     * @param startTime
     * @param endTime
     * @param isEvent
     * @param callback
     * @param playFinishedCallBack
     */
     void playCloudDataWithStartTime(long startTime, long endTime, boolean isEvent, OperationCallBack callback, OperationCallBack playFinishedCallBack);

    /**
     * 暂停云存储视频
     */
    void pausePlayCloudVideo();

    /**
     * 恢复云存储视频
     */
    void resumePlayCloudVideo();

    /**
     * 结束云存储播放
     */
    void stopPlayCloudVideo();

       /**
     * 设置音频输出
     */
    void cloudSetMute(Context context, int mute, OperationCallBack callback);

    void cloudGetMute(OperationCallBack callback);

    /**
     * 截屏操作
     *
     * @param context
     * @param filePath
     */
    void cloudSnapshot(Context context, String filePath, OperationCallBack callback);

    /**
     * 开始录制mp4
     *
     * @param context
     * @param folderPath
     * @param fileName
     */
    void cloudStartRecordMp4(Context context, String folderPath, String fileName, OperationCallBack callback);

    /**
     * 结束录制mp4
     * @param callback
     */
    void cloudStopRecordMp4(OperationCallBack callback);

	/**
	* 获取日期数据
	*/
    List<CloudDayBean> getDayBeanList();

    /**
     * 销毁
     */
    void onDestroy();
}

```



#### 获取对象

```java
private ICloudCameraManager cloudCameraManager;
cloudCameraManager = TuyaSmartCloudVideoPlayer.getInstance();
```



#### 回调

ICloudManagerCallback 接口提供了监听云存储日期、事件、时间片段数据。

```java
  public interface ICloudManagerCallback<T> {

        void onError(int errorCode);

        void onSuccess(T o, int code);
    }
```

OnCloudCameraVideoListener 接口提供了云存储视频YUV数据回调

```java
public interface OnCloudCameraVideoListener {

    void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera);
    
}

```

OperationCallBack 接口提供了云存储视频操作的回调

```java
public interface OperationCallBack {
    //操作成功
    void onSuccess(int sessionId, int requestId, String data, Object camera);
	//操作失败
    void onFailure(int sessionId, int requestId, int errCode, Object camera);
}
```



## 使用方法



#### 云存储视频播放器

TuyaMonitorView 类是云存储播放器，提供的是视频播放的功能，与 IPC Camera 摄像机的播放器是同一个。

**【创建对象】**

```java
//播放器view最好宽高比设置16:9
        WindowManager windowManager = (WindowManager) this.getSystemService(WINDOW_SERVICE);
        int width = windowManager.getDefaultDisplay().getWidth();
        int height = width * ASPECT_RATIO_WIDTH / ASPECT_RATIO_HEIGHT;
        LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(width, height);
        monitorView = findViewById(R.id.tmv_video_view);
        monitorView.createVideoView(2,true);
        monitorView.setLayoutParams(layoutParams);
```



#### 初始化云存储

参数1：DeviceBean ，参数2：ICloudManagerCallback ，如果回调成功的话，就可以获取云存储日期数据。

**参数说明**

| 参数       | 描述                                                |
| ---------- | --------------------------------------------------- |
| deviceBean | DeviceBean，设备 bean                               |
| callback   | ICloudManagerCallback，回调成功，获取云存储日期数据 |

**callback 状态码**

```java
		public static final int NO_SERVES = 10001; //无云服务
    public static final int SERVES_NO_CLOUD_DATA = 10002;  //有服务无云存储数据
    public static final int SERVES_DATA = 10003;    //有云服务且有云存储数据
    public static final int EXPIRED_SERVES_DATA = 10004;   //云服务过期且有云存储数据
    public static final int EXPIRED_SERVES_NO_CLOUD_DATA = 10005;   //云服务过期且没有云储存数据
    public static final int ERROR_QUERY_CODE = 10006;      //数据查询失败
```

**示例代码**

```java
/**
  * 初始化云存储
 */
private void initCameraCloud() {
        DeviceBean deviceBean = TuyaHomeSdk.getDataInstance().getDeviceBean(devId);
        cloudCameraManager = TuyaSmartCloudVideoPlayer.getInstance();
        cloudCameraManager.registorOnCloudCameraVideoListener(this);
    	//云存储初始化
        cloudCameraManager.initCameraCloud(deviceBean, new 			   CloudCameraManager.ICloudManagerCallback() {
                    @Override
                    public void onError(int i) {
                        Log.d(TAG, "initCameraCloud onError errcode " + i);
                    }

                    @Override
                    public void onSuccess(Object o, int i) {
                        Log.d(TAG, "initCameraCloud onSuccess");
                        queryDayList = cloudCameraManager.getDayBeanList();
                        mDaysAdapter.updateData(queryDayList);
                    }
                }
        );
    }
```

> 注意：云存储初始化非常重要，如果出现 ERROR_QUERY_CODE，要重新初始化

#### 云存储日期数据查询

```java
List<CloudDayBean> queryDayList = cloudCameraManager.getDayBeanList();
```

**【数据结构】**

```java
public class CloudDayBean implements Comparable<CloudDayBean> {

    private int sumDuration;   //总时长（忽略）
    private String uploadDay;   //上传时间（忽略）
    private long currentDayStart; //该天开始的时间
    private long currentDayEnd;   //该天结束的时间

    public int getSumDuration() {
        return sumDuration;
    }

    public void setSumDuration(int sumDuration) {
        this.sumDuration = sumDuration;
    }

    public String getUploadDay() {
        return uploadDay;
    }

    public void setUploadDay(String uploadDay) {
        this.uploadDay = uploadDay;
    }

    public String getMonthAndDay() {
        if (uploadDay != null) {
            return uploadDay.substring(5).replace("-", ".");
        }
        return null;
    }

    public long getCurrentStartDayTime() {
        currentDayStart = CalendarUtils.getTodayStart(CalendarUtils.parse(uploadDay, FORMAT_SHORT));
        return currentDayStart;
    }

    public long getCurrentDayEndTime() {
        currentDayEnd = CalendarUtils.getTodayEnd(CalendarUtils.parse(uploadDay, FORMAT_SHORT));
        return currentDayEnd;
    }

    @Override
    public int compareTo(@NonNull CloudDayBean o) {
        ...
    }
}
```



#### 云存储时间片段数据查询

cloudCameraManager.getCloudTimeLine 通过回调获取时间片段的数据，获取到的数据是以 jsonArray 形式输出来的。

**参数说明**

| 参数     | 描述     |
| -------- | -------- |
| devId    | 设备 id  |
| timeGT   | 开始时间 |
| timeLT   | 结束时间 |
| callback | 回调     |

callback 状态码：

```java
public static final int QUERY_SUCCESS = 10000; //数据查询成功
public static final int ERROR_QUERY_CODE = 10006;      //数据查询失败
public static final int NO_TIME_PIECE_DATA = 10008; //无云存储时间片段数据
```

TimePieceBean 是时间片段的 bean 类

**数据结构**：

```java
public class TimePieceBean {
    private int startTime; //开始时间
    private int endTime;   //结束时间
    private int playTime;  //播放时间
    private int prefix;    //前缀

    public TimePieceBean() {
    }



    public long getStartTimeInMillisecond() {
        return startTime * 1000L;
    }

    public long getEndTimeInMillisecond() {
        return endTime * 1000L;
    }

    public int getStartTime() {
        return startTime;
    }

    public void setStartTime(int startTime) {
        this.startTime = startTime;
    }

    public int getEndTime() {
        return endTime;
    }

    public void setEndTime(int endTime) {
        this.endTime = endTime;
    }

    public int getPlayTime() {
        return playTime;
    }

    public void setPlayTime(int playTime) {
        this.playTime = playTime;
    }

    public int getPrefix() {
        return prefix;
    }

    public void setPrefix(int prefix) {
        this.prefix = prefix;
    }
}

```

**【使用方法】**

```java
cloudCameraManager.getCloudTimeLine(devId, String.valueOf(cloudDayBean.getCurrentStartDayTime()), String.valueOf(cloudDayBean.getCurrentDayEndTime()), new CloudCameraManager.ICloudManagerCallback<JSONArray>() {

                    @Override
                    public void onError(int i) {

                    }

                    @Override
                    public void onSuccess(JSONArray jsonArray, int i) {
                        List<TimePieceBean> timePieceList = JSONArray.parseArray(jsonArray.toJSONString(), TimePieceBean.class);
                        mTimeLineadapter.updateData(timePieceList);
                    }
                });
```

> 当状态码为：ERROR_QUERY_CODE，需要重新查询。

#### 云存储事件数据查询

cloudCameraManager.getTimeEvent() 通过回调获取时间事件数据，获取到成功的数据是以 jsonArray 形式输出来的。

**参数说明**

| 参数         | 描述                        |
| ------------ | --------------------------- |
| CloudDayBean | 日期数据 bean               |
| devId        | 设备 id                     |
| offset       | 数据起始查询点(一般设置为0) |
| limit        | 查询条数                    |
| callback     | 回调                        |

callback 状态码：

```java
public static final int ERROR_QUERY_CODE = 10006;      //数据查询失败
public static final int QUERY_SUCCESS = 10000; //数据查询成功
public static final int NO_TIME_RANG_DATA = 10007; //无云存储事件数据
```



TimeEventBean 是一个事件的数据 bean 类。

**【数据结构】**

```java
public class TimeEventBean {
    private String describe;  //描述
    private int endTime;   //结束时间
    private String snapshotUrl; //图片地址
    private int startTime;  //开始时间
    private int v;

    private STATUS status = STATUS.UN_SELECT;

    public STATUS getStatus() {
        return status;
    }

    public void setStatus(STATUS status) {
        this.status = status;
    }

    public String getDescribe() {
        return describe;
    }

    public void setDescribe(String describe) {
        this.describe = describe;
    }

    public int getEndTime() {
        return endTime;
    }

    public void setEndTime(int endTime) {
        this.endTime = endTime;
    }

    public String getSnapshotUrl() {
        return snapshotUrl;
    }

    public void setSnapshotUrl(String snapshotUrl) {
        this.snapshotUrl = snapshotUrl;
    }

    public int getStartTime() {
        return startTime;
    }

    public void setStartTime(int startTime) {
        this.startTime = startTime;
    }

    public int getV() {
        return v;
    }

    public void setV(int v) {
        this.v = v;
    }

    public enum STATUS {
        SELECT, UN_SELECT
    }
}
```



```java
cloudCameraManager.getTimeEvent(cloudDayBean, devId, 0, -1, new CloudCameraManager.ICloudManagerCallback<JSONArray>() {
                    @Override
                    public void onError(int i) {

                    }

                    @Override
                    public void onSuccess(JSONArray o, int i) {
             			List<TimeEventBean> mTimeRangeList = 															JSONArray.parseArray(o.toJSONString(), 														TimeEventBean.class);
                   
                    }
                });
```

> 当状态码为：ERROR_QUERY_CODE，需要重新查询。

#### 云存储视频播放相关操作

**【开始播放】**

| 参数                 | 描述                                                |
| -------------------- | --------------------------------------------------- |
| startTime            | 开始时间                                            |
| endTime              | 结束时间                                            |
| isEvent              | 是否是事件数据(true是事件数据，false是时间片段数据) |
| callback             | 播放操作的回调                                      |
| playFinishedCallBack | 播放结束回调                                        |

事例代码：

```java
if (null != timePieceBean) {
                  cloudCameraManager.playCloudDataWithStartTime(timePieceBean.getStartTime(), timePieceBean.getEndTime(), false, new OperationCallBack() {
                        @Override
                        public void onSuccess(int i, int i1, String s, Object o) {
							//操作成功
                        }

                        @Override
                        public void onFailure(int i, int i1, int i2, Object o) {
							//操作失败
                        }
                    }, new OperationCallBack() {
                        @Override
                        public void onSuccess(int i, int i1, String s, Object o) {

                        }

                        @Override
                        public void onFailure(int i, int i1, int i2, Object o) {

                        }
                    });
                }
```



**【暂停播放】**

```java
cloudCameraManager.pausePlayCloudVideo(new OperationCallBack() {
                    @Override
                    public void onSuccess(int i, int i1, String s, Object o) {
                        Log.d(TAG,"onSuccess");
                    }

                    @Override
                    public void onFailure(int i, int i1, int i2, Object o) {
                        Log.d(TAG,"onFailure");
                    }
                });
```



**【恢复播放】**

```java
cloudCameraManager.resumePlayCloudVideo(new OperationCallBack() {
                    @Override
                    public void onSuccess(int i, int i1, String s, Object o) {
                        Log.d(TAG,"onSuccess");
                    }

                    @Override
                    public void onFailure(int i, int i1, int i2, Object o) {
                        Log.d(TAG,"onFailure");
                    }
                });
```



**【结束播放】**

```java
cloudCameraManager.stopPlayCloudVideo(new OperationCallBack() {
                    @Override
                    public void onSuccess(int i, int i1, String s, Object o) {
                        Log.d(TAG,"onSuccess");
                    }

                    @Override
                    public void onFailure(int i, int i1, int i2, Object o) {
                        Log.d(TAG,"onFailure");
                    }
                });
```

> 注意：一旦结束了播放，就不能恢复播放，要重新调用 playCloudDataWithStartTime 开始播放



**【音频设置】**

muteValue = 0   代表的是静音

muteValue = 1 代表的是非静音

```java
final int muteValue = 0;
                cloudCameraManager.cloudSetMute(CameraCloudActivity.this, muteValue, new OperationCallBack() {
                    @Override
                    public void onSuccess(int i, int i1, String s, Object o) {
                        if (muteValue == 1) {
                            	ivMute.setImageResource(R.drawable.camera_preview_sound_btn_off);
                        } else if (muteValue == 0) {
                            ivMute.setImageResource(R.drawable.camera_preview_sound_btn_on);
                        }
                    }
```



**【获取音频开关状态】**

muteValue = 0   代表的是静音

muteValue = 1 代表的是非静音

```java
    private void getmute() {
        int muteValue = cloudCameraManager.cloudGetMute();
        if (muteValue == 1) {
            ivMute.setImageResource(R.drawable.camera_preview_sound_btn_off);
        } else if (muteValue == 0) {
            ivMute.setImageResource(R.drawable.camera_preview_sound_btn_on);
        }
    }
```



**【截屏操作】**

| 参数              | 描述     |
| ----------------- | -------- |
| Context           | 上下文   |
| Path              | 文件路径 |
| OperationCallBack | 操作回调 |

```java
String path = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
                File file = new File(path);
                if (!file.exists()) {
                    file.mkdirs();
                }
                path = path + System.currentTimeMillis() + ".png";
                cloudCameraManager.cloudSnapshot(CameraCloudActivity.this, path, new OperationCallBack() {
                    @Override
                    public void onSuccess(int i, int i1, String s, Object o) {
                        Log.d(TAG, "snapshot onSuccess");
                    }

                    @Override
                    public void onFailure(int i, int i1, int i2, Object o) {
                        Log.d(TAG, "snapshot onFailure");
                    }
                });
```

**【开始录制mp4】**

| 参数              | 描述       |
| ----------------- | ---------- |
| Context           | 上下文     |
| Path              | 文件夹路径 |
| fileName          | 文件名称   |
| OperationCallBack | 操作回调   |

```java
String path = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
                    File file = new File(path);
                    if (!file.exists()) {
                        file.mkdirs();
                    }
                    String fileName = System.currentTimeMillis() + ".mp4";
                    cloudCameraManager.startCloudRecordMp4(CameraCloudActivity.this, path, fileName, new OperationCallBack() {
                        @Override
                        public void onSuccess(int i, int i1, String s, Object o) {
                            isRecord = true;
                            Log.d(TAG, "startCloudRecordMp4 onSuccess");
                            recordBtn.setText("stop record");
                        }

                        @Override
                        public void onFailure(int i, int i1, int i2, Object o) {
                            Log.d(TAG, "startCloudRecordMp4 onFailure");
                        }
                    });
```



**【结束录制mp4】**

| 参数              | 描述     |
| ----------------- | -------- |
| OperationCallBack | 操作回调 |

```java
cloudCameraManager.stopCloudRecordMp4(new OperationCallBack() {
                        @Override
                        public void onSuccess(int i, int i1, String s, Object o) {
                            Log.d(TAG, "stopCloudRecordMp4 onSuccess");
                            recordBtn.setText("start record");
                        }

                        @Override
                        public void onFailure(int i, int i1, int i2, Object o) {
                            Log.d(TAG, "stopCloudRecordMp4 onFailure");
                        }
                    });
```



**【视频数据回调】**

```java
  @Override
    public void onReceiveFrameYUVData(int i, ByteBuffer byteBuffer, ByteBuffer byteBuffer1, ByteBuffer byteBuffer2, int i1, int i2, long l, Object o) {
        monitorView.receiveFrameYUVData(i, byteBuffer, byteBuffer1, byteBuffer2, i1, i2, l);
    }
```

