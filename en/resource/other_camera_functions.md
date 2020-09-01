# Other methods invocation



### Turn on video recording


int startRecordLocalMp4(String folderPath, String fileName, Context context, OperationDelegateCallBack callBack);


**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| folderPath |yes|string| File path|
| fileName | yes |string| File name|
| Context | yes || |
| OperationDelegateCallBack | yes || operation callback |

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

> tip: video recording requires to write memory card permission,Suggestions for processing in sub-threads


### Stop video recording

int stopRecordLocalMp4(OperationDelegateCallBack callBack);

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| OperationDelegateCallBack ||| operation callback|

```java
  mCameraP2P.stopRecordLocalMp4(new OperationDelegateCallBack() {
                @Override
                public void onSuccess(int sessionId, int requestId, String data) {
                    isRecording = false;
                    //data: Returns the file path
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_VIDEO_RECORD_OVER, ARG1_OPERATE_SUCCESS, data));
                }

                @Override
                public void onFailure(int sessionId, int requestId, int errCode) {
                    isRecording = false;
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_VIDEO_RECORD_OVER, ARG1_OPERATE_FAIL));
                }
            });
```

> tip: video will be saved in custom routine once it recorded successfully.



### video screenshot

int snapshot(String absoluteFilePath, Context context, PLAYMODE playmode, OperationDelegateCallBack callBack);

video screen shot  needs to be divided by live mode and playback mode

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| absoluteFilePath ||string| File path|
| Context ||| |
| PLAYMODE ||| live mode / playback mode|
| OperationDelegateCallBack ||| operation callback|

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
             //data Returns the file path
                mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_SUCCESS, data));
            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {
                mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_FAIL));
            }
        });
```


### set microphone status
void setMute(PLAYMODE playModel, int mute, OperationDelegateCallBack callBack);

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| PLAYMODE ||| live mode / playback mode|
| mute |||  MUTE/UNMUTE |
| OperationDelegateCallBack ||| operation callback|

```java
  int mute;
        mute = previewMute == ICameraP2P.MUTE ? ICameraP2P.UNMUTE : ICameraP2P.MUTE;
        mCameraP2P.setMute(ICameraP2P.PLAYMODE.LIVE, mute, new OperationDelegateCallBack() {
            @Override
            public void onSuccess(int sessionId, int requestId, String data) {
            		//data： Return the result value after the corresponding operation
                previewMute = Integer.valueOf(data);
                mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_SUCCESS));
            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {
                mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_FAIL));
            }
        });
```



### open intercom

void startAudioTalk(OperationDelegateCallBack callBack);

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| OperationDelegateCallBack ||| operation callback|


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





### Stop intercom

int stopAudioTalk(OperationDelegateCallBack callBack)

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| OperationDelegateCallBack ||| operation callback|


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

> tip: intercom and recording are exclusive, intercom can be opened only during live view



### query clarity

void getVideoClarity(OperationDelegateCallBack callBack);

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| OperationDelegateCallBack ||| operation callback|


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
> Note: After the preview screen comes out, call the function


### Change clarity

void setVideoClarity(int clarity, OperationDelegateCallBack callback);

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| mode ||| HD / STANDEND |
| OperationDelegateCallBack ||| operation callback|


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


  definition mode

 ```java
  public interface ICameraP2P<T> {
       /**
       *  HD
       */
      int HD = 4;
      /**
       *  SD
       */
      int STANDEND = 2;
      ...
          
      }

 ```



###  Get the dates that have playback video recording info


void queryRecordDaysByMonth(int year, int month, OperationDelegateCallBack callBack);

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| year ||int| |
| month ||int| |
| OperationDelegateCallBack ||| operation callback|	


> Need to get playback video recording info before playback. First, get the dates that have playback video recording info.

  ```java
  
  int year = Integer.parseInt(substring[0]);
                    int mouth = Integer.parseInt(substring[1]);
                    queryDay = Integer.parseInt(substring[2]);
                    mCameraP2P.queryRecordDaysByMonth(year, mouth, new OperationDelegateCallBack() {
                        @Override
                        public void onSuccess(int sessionId, int requestId, String data) {
                        //data: Obtain date data by year and month
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



### Get the video playback info of someday

void queryRecordTimeSliceByDay(int year, int month, int day, OperationDelegateCallBack callBack);

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| year ||int| |
| month ||int| |
| day ||int| |
| OperationDelegateCallBack ||| operation callback|	

> After getting the dates that have playback recording, query playback data by day.

```java
int year = Integer.parseInt(substring[0]);
                int mouth = Integer.parseInt(substring[1]);
                int day = Integer.parseInt(substring[2]);
                mCameraP2P.queryRecordTimeSliceByDay(year, mouth, day, new OperationDelegateCallBack() {
                    @Override
                    public void onSuccess(int sessionId, int requestId, String data) {						    //data: Obtained date-time fragment data
                        parsePlaybackData(data);
                    }

                    @Override
                    public void onFailure(int sessionId, int requestId, int errCode) {
                        mHandler.sendEmptyMessage(MSG_DATA_DATE_BY_DAY_FAIL);
                    }
                });
```

### Start playback

void startPlayBack(int startTime, int stopTime,
                       int playTime, OperationDelegateCallBack callBack, OperationDelegateCallBack finishcallBack);
                       
**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| startTime ||int| time-slice of start Time|
| stopTime ||int| time-slice of end Time|
| playTime ||int| time-slice of play Time，play Time between start Time and End Time |
| OperationDelegateCallBack ||| operation callback|	
| OperationDelegateCallBack |||  finish playback operation callback|   
                       

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

 ### Pause playback
 
 **parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| OperationDelegateCallBack ||| pause playback operation callback|

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

### Resume playback
void resumePlayBack(OperationDelegateCallBack callBack)

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| OperationDelegateCallBack ||| resume playback operation callback|

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

### Playback end callback

void stopPlayBack(OperationDelegateCallBack callBack);

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| OperationDelegateCallBack ||| stop playback operation callback|

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



###  Get streaming video data



  ```java

      @Override
      public void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera) {
          mVideoView.receiveFrameYUVData(sessionId, y, u, v, width, height, camera);
      }
  	
  ```

**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| sessionId || int | |
| y || ByteBuffer | Y Data|
| u || ByteBuffer | U Data|
| v || ByteBuffer | V Data|
| width || int | video Frame width |
| height || int | video Frame height |
| nFrameRate || int | FrameRate|
| nIsKeyFrame || int | |
| timestamp || int | Frame TimeStamp|
| nProgress || int | |
| nDuration || int | |



###  Get p2p status
> passive disconnect callback 

```java

@Override
public void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus) {
Log.d(TAG, "sessionId " + sessionId + "sessionStatus " + sessionStatus);
}

```


**parameter：**

|parameter|Mandatory|Type|Description|
|:----    |:---|:----- |-----   |
| sessionId || int | |
