

# 其他方法调用



### 开启视频录制
```java
int startRecordLocalMp4(String folderPath, String fileName, Context context, OperationDelegateCallBack callBack);
```
是阻塞方法，建议在子线程中做处理

**参数说明**

|参数|必选|类型|说明|
|:----    |:---|:----- |-----   |
| folderPath |是|string| 文件路径|
| fileName |是|string| 文件名称|
| Context |是|| 上下文|
| OperationDelegateCallBack |是|| 操作回调|

**示例代码**

```java
 if (Constants.hasStoragePermission()) {
                String picPath = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
                File file = new File(picPath);
                if (!file.exists()) {
                    file.mkdirs();
                }
                String fileName = System.currentTimeMillis() + ".mp4";
                videoPath = picPath + fileName;
                mCameraP2P.startRecordLocalMp4(picPath, fileName, CameraPanelActivity.this, new OperationDelegateCallBack() {
                    @Override
                    public void onSuccess(int sessionId, int requestId, String data) {
                        isRecording = true;
                        mHandler.sendEmptyMessage(MSG_VIDEO_RECORD_BEGIN);

                    }

                    @Override
                    public void onFailure(int sessionId, int requestId, int errCode) {
                        mHandler.sendEmptyMessage(MSG_VIDEO_RECORD_FAIL);
                    }
                });
                recordStatue(true);
            } else {
                Constants.requestPermission(CameraPanelActivity.this, Manifest.permission.WRITE_EXTERNAL_STORAGE, Constants.EXTERNAL_STORAGE_REQ_CODE, "open_storage");
            }     
```

> 注：录制视频需要写存储卡权限



### 停止视频录制
```
int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```
**参数说明**

|参数|必选|说明|
|:----    |:---|-----   |
| OperationDelegateCallBack || 操作回调|

**示例代码**
```java
  mCameraP2P.stopRecordLocalMp4(new OperationDelegateCallBack() {
                @Override
                public void onSuccess(int sessionId, int requestId, String data) {
                    isRecording = false;
                    //data 返回的是文件路径
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_VIDEO_RECORD_OVER, ARG1_OPERATE_SUCCESS, data));
                }

                @Override
                public void onFailure(int sessionId, int requestId, int errCode) {
                    isRecording = false;
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_VIDEO_RECORD_OVER, ARG1_OPERATE_FAIL));
                }
            });
```

> 注：如果视频录制成功，会保存在自定义的路径当中



### 视频截图
```java
int snapshot(String absoluteFilePath, Context context, PLAYMODE playmode, OperationDelegateCallBack callBack);
```

视频截图需要区分是 live、playback 模式
	
**参数说明**

|参数|必选|类型|说明|
|:----    |:---|:----- |-----   |
| absoluteFilePath ||string| 文件路径|
| Context ||| 上下文|
| PLAYMODE ||| 播放模式：live、playback 模式 |
| OperationDelegateCallBack ||| 操作回调|

**示例代码**
```java
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            String path = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
            File file = new File(path);
            if (!file.exists()) {
                file.mkdirs();
            }
            picPath = path;
        }
        mCameraP2P.snapshot(picPath, CameraPanelActivity.this, ICameraP2P.PLAYMODE.LIVE, new OperationDelegateCallBack() {
            @Override
            public void onSuccess(int sessionId, int requestId, String data) {
             //data 返回的是文件路径
                mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_SUCCESS, data));
            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {
                mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_FAIL));
            }
        });
```


### 设置拾音器状态
```java
void setMute(PLAYMODE playModel, int mute, OperationDelegateCallBack callBack);
```

**参数说明**

|参数|必选|类型|说明|
|:----    |:---|:----- |-----   |
| PLAYMODE ||| 播放模式：live、playback 模式 |
| mute ||| 拾音器模式：MUTE/UNMUTE（静音/非静音） |
| OperationDelegateCallBack ||| 操作回调|

**示例代码**
```java
  int mute;
        mute = previewMute == ICameraP2P.MUTE ? ICameraP2P.UNMUTE : ICameraP2P.MUTE;
        mCameraP2P.setMute(ICameraP2P.PLAYMODE.LIVE, mute, new OperationDelegateCallBack() {
            @Override
            public void onSuccess(int sessionId, int requestId, String data) {
            		//data返回的是对应操作之后的结果值
                previewMute = Integer.valueOf(data);
                mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_SUCCESS));
            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {
                mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_FAIL));
            }
        });
```




### 开启对讲
```java
void startAudioTalk(OperationDelegateCallBack callBack);
```

**参数说明**

|参数|必选|说明|
|:----    |:---|-----   |
| OperationDelegateCallBack || 操作回调|

**示例代码**
```java
  if (Constants.hasRecordPermission()) {
                mCameraP2P.startAudioTalk(new OperationDelegateCallBack() {
                    @Override
                    public void onSuccess(int sessionId, int requestId, String data) {
                        isSpeaking = true;
                        mHandler.sendMessage(MessageUtil.getMessage(MSG_TALK_BACK_BEGIN, ARG1_OPERATE_SUCCESS));
                        ToastUtil.shortToast(CameraPanelActivity.this, "start talk success");
                    }

                    @Override
                    public void onFailure(int sessionId, int requestId, int errCode) {
                        isSpeaking = false;
                        mHandler.sendMessage(MessageUtil.getMessage(MSG_TALK_BACK_BEGIN, ARG1_OPERATE_FAIL));
                        ToastUtil.shortToast(CameraPanelActivity.this, "operation fail");

                    }
                });
            } else {
                Constants.requestPermission(CameraPanelActivity.this, Manifest.permission.RECORD_AUDIO, Constants.EXTERNAL_AUDIO_REQ_CODE, "open_recording");
            }
```

### 停止对讲
```java
int stopAudioTalk(OperationDelegateCallBack callBack)
```
**参数说明**

|参数|必选|说明|
|:----    |:---|-----   |
| OperationDelegateCallBack || 操作回调|

**示例代码**
 ```java
   mCameraP2P.stopAudioTalk(new OperationDelegateCallBack() {
                @Override
                public void onSuccess(int sessionId, int requestId, String data) {
                    isSpeaking = false;
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_TALK_BACK_OVER, ARG1_OPERATE_SUCCESS));
                }

                @Override
                public void onFailure(int sessionId, int requestId, int errCode) {
                    isSpeaking = false;
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_TALK_BACK_OVER, ARG1_OPERATE_FAIL));

                }
            });
 ```

> 注：对讲和录制是互斥的，而且只有在预览时可以开启对讲。




### 获取清晰度
```java
void getVideoClarity(OperationDelegateCallBack callBack);
```
**参数说明**

|参数名|必选|说明|
|:----    |:---|-----   |
| OperationDelegateCallBack || 操作回调|

**示例代码**
```java
	mCameraP2P.getVideoClarity(new OperationDelegateCallBack() {
            @Override
            public void onSuccess(int sessionId, int requestId, String data) {
                
            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {

            }
        });
```

> 注意：预览画面出来后，调取该函数

### 分辨率改变

**参数说明**

|参数|必选|类型|说明|
|:----    |:---|:----- |-----   |
| mode ||| 清晰度模式|
| OperationDelegateCallBack ||| 操作回调|

**示例代码**
```java

     mCameraP2P.setVideoClarity(videoClarity == ICameraP2P.HD ? ICameraP2P.STANDEND : ICameraP2P.HD, new OperationDelegateCallBack() {
            @Override
            public void onSuccess(int sessionId, int requestId, String data) {
                videoClarity = Integer.valueOf(data);
                mHandler.sendMessage(MessageUtil.getMessage(MSG_GET_CLARITY, ARG1_OPERATE_SUCCESS));
            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {
                mHandler.sendMessage(MessageUtil.getMessage(MSG_GET_CLARITY, ARG1_OPERATE_FAIL));
            }
        });
```

清晰度模式

 ```java
public interface ICameraP2P<T> {
  /**
   * 高清
   */
  int HD = 4;
  /**
   * 标清
   */
  int STANDEND = 2;
  ...
  }

 ```


###  获取有回放视频记录的日期
```java
void queryRecordDaysByMonth(int year, int month, OperationDelegateCallBack callBack);
```
**参数说明**

|参数|必选|类型|说明|
|:----    |:---|:----- |-----   |
| year ||int| 年|
| month ||int| 月|
| OperationDelegateCallBack ||| 操作回调|

> 在开始回放前，需要获取到回放视频记录的信息。首先获取有回放视频记录的日期

**示例代码**

```java
		int year = Integer.parseInt(substring[0]);
    int mouth = Integer.parseInt(substring[1]);
    queryDay = Integer.parseInt(substring[2]);
    mCameraP2P.queryRecordDaysByMonth(year, mouth, new OperationDelegateCallBack() {
                        @Override
                        public void onSuccess(int sessionId, int requestId, String data) {
                        //data是获取到的月份数据
                            MonthDays monthDays = JSONObject.parseObject(data, MonthDays.class);
                            mBackDataMonthCache.put(mCameraP2P.getMonthKey(), monthDays.getDataDays());
                            mHandler.sendMessage(MessageUtil.getMessage(MSG_DATA_DATE, ARG1_OPERATE_SUCCESS, data));
                        }

                        @Override
                        public void onFailure(int sessionId, int requestId, int errCode) {
                            mHandler.sendMessage(MessageUtil.getMessage(MSG_DATA_DATE, ARG1_OPERATE_FAIL));
                        }
                    }); 
```



### 获取某日的视频回放信息
```
void queryRecordTimeSliceByDay(int year, int month, int day, OperationDelegateCallBack callBack);
```

**参数说明**

|参数|必选|类型|说明|
|:----    |:---|:----- |-----   |
| year ||int| 年|
| month ||int| 月|
| day ||int| 日|
| OperationDelegateCallBack ||| 操作回调|

 > 获取到有用回放记录的日期后，根据日期获取当日的视频回放记录

**示例代码**

```java
int year = Integer.parseInt(substring[0]);
                int mouth = Integer.parseInt(substring[1]);
                int day = Integer.parseInt(substring[2]);
                mCameraP2P.queryRecordTimeSliceByDay(year, mouth, day, new OperationDelegateCallBack() {
                    @Override
                    public void onSuccess(int sessionId, int requestId, String data) {					//data是获取到的日期时间片段数据
                        parsePlaybackData(data);
                    }

                    @Override
                    public void onFailure(int sessionId, int requestId, int errCode) {
                        mHandler.sendEmptyMessage(MSG_DATA_DATE_BY_DAY_FAIL);
                    }
                });
```


### 开启回放
```java
void startPlayBack(int startTime, int stopTime,
                       int playTime, OperationDelegateCallBack callBack, OperationDelegateCallBack finishcallBack);
```

**参数说明**

|参数|必选|类型|说明|
|:----    |:---|:----- |-----   |
| startTime ||int| 录像片段的开始时间|
| stopTime ||int| 录像片段的结束时间|
| playTime ||int| 录像片段的播放时间，播放时间必须在开始时间和结束时间之间|
| OperationDelegateCallBack ||| 操作回调|
| OperationDelegateCallBack ||| 结束视频播放回调|

**示例代码**
  ``` java
mCameraP2P.startPlayBack(timePieceBean.getStartTime(),
                         timePieceBean.getEndTime(),
                         timePieceBean.getStartTime(), new OperationDelegateCallBack() {
                           @Override
                           public void onSuccess(int sessionId, int requestId, String data) {
                             isPlayback = true;
                           }

                           @Override
                           public void onFailure(int sessionId, int requestId, int errCode) {
                             isPlayback = false;
                           }
                         }, new OperationDelegateCallBack() {
                           @Override
                           public void onSuccess(int sessionId, int requestId, String data) {
                             isPlayback = false;
                           }

                           @Override
                           public void onFailure(int sessionId, int requestId, int errCode) {
                             isPlayback = false;
                           }
                         });
       
  ```



### 暂停回放

```java
void pausePlayBack(OperationDelegateCallBack callBack);
```

**参数说明**

|参数|必选|说明|
|:----    |:---|-----   |
| OperationDelegateCallBack || 暂停播放回放的操作回调|

**示例代码**
  ```java
mCameraP2P.pausePlayBack(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    isPlayback = false;
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
  ```

### 恢复回放
```java
void resumePlayBack(OperationDelegateCallBack callBack)
```
**参数说明**

|参数|必选|说明|
|:----    |:---|-----   |
| OperationDelegateCallBack || 恢复播放回放的操作回调|

**示例代码**
```java
mCameraP2P.resumePlayBack(new OperationDelegateCallBack() {
            @Override
            public void onSuccess(int sessionId, int requestId, String data) {
                isPlayback = true;
            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {

            }
        });
```


### 回放结束回调
```java
void stopPlayBack(OperationDelegateCallBack callBack);
```

**参数说明**

|参数|必选|说明|
|:----    |:---|-----   |
| OperationDelegateCallBack || 结束播放回放的操作回调|

**示例代码**
```java
	mCameraP2P.stopPlayBack(new OperationDelegateCallBack() {
            @Override
            public void onSuccess(int sessionId, int requestId, String data) {

            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {

            }
        });
```


### 获取视频流数据

> 注意：该方法仅为 p2ptype=2 的设备类型使用

```java
void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, int nFrameRate, int nIsKeyFrame, long timestamp, long nProgress, long nDuration, Object camera)
```

**参数说明**

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
| sessionId || int | p2p 连接的 sessionId，无需关注 |
| y || ByteBuffer | 视频帧Y数据|
| u || ByteBuffer | 视频帧U数据|
| v || ByteBuffer | 视频帧V数据|
| width || int | 视频画面的宽度|
| height || int | 视频画面的高度|
| nFrameRate || int | 视频帧率|
| nIsKeyFrame || int | 无需关注|
| timestamp || int | 视频时间戳|
| nProgress || int | 无需关注|
| nDuration || int | 无需关注|

**示例代码**
```java
@Override
public void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, int nFrameRate, int nIsKeyFrame, long timestamp, long nProgress, long nDuration, Object camera) {}
```
### 获取p2p状态

> 注意：该方法仅为 p2ptype=2的设备类型使用，当收到该函数的回调，代表着 p2p 断开

```java       
void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus)
```
**参数说明**

|参数|必选|类型|说明|
|:----    |:---|:----- |-----   |
| sessionId || int | p2p 连接的 sessionId |

**示例代码**  

```java       
@Override
public void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus) {
  Log.d(TAG, "sessionId " + sessionId + "sessionStatus " + sessionStatus);
}
```