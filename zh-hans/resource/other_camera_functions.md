# 其他方法调用



### 开启视频录制

这里有两个方法可以调用,都是阻塞方法，建议在子线程中做处理

- 不带有音频数据的录像方法

  ```java
  if (Constants.hasStoragePermission()) {
                String picPath = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
                File file = new File(picPath);
                if (!file.exists()) {
                    file.mkdirs();
                }
                String fileName = System.currentTimeMillis() + ".mp4";
		
                int ret = camera.startRecordLocalMp4WithoutAudio(picPath, fileName, this);    // ret =0 success, ret < 0 failure
            } else { 
                Constants.requestPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE, Constants.EXTERNAL_STORAGE_REQ_CODE, "open_storage");
            }
	          
  ```

- 带有音频数据的录像方法

```java
 if (Constants.hasStoragePermission()) {
                String picPath = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
                File file = new File(picPath);
                if (!file.exists()) {
                    file.mkdirs();
                }
                String fileName = System.currentTimeMillis() + ".mp4";
		
                int ret = camera.startRecordLocalMp4(picPath, fileName, this);    // ret =0 success, ret < 0 failure
            } else { 
                Constants.requestPermission(this, Manifest.permission.WRITE_EXTERNAL_STORAGE, Constants.EXTERNAL_STORAGE_REQ_CODE, "open_storage");
            }     
```

> 注：录制视频需要写存储卡权限,p2pType=2的设备，暂时不支持startRecordLocalMp4该方法



### 停止视频录制

```java
  int isStopRecord = -1;
              try {
                  isStopRecord = camera.stopRecordLocalMp4();
              } catch (Exception e) {
                  e.printStackTrace();
              }
              if (isStopRecord == 0) {
                  Toast.makeText(MainActivity.this, "录像结束。。。", Toast.LENGTH_SHORT).show();
              } else {
                  Toast.makeText(MainActivity.this, "录像结束失败。。。", Toast.LENGTH_SHORT).show();
              }
```

> 注：如果视频录制成功，会保存在自定义的路径当中



### 视频截图

视频截图需要区分是live、playback模式

```java
  String picPath = null;
              if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
                  picPath = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/" + System.currentTimeMillis() + ".png";
              }
              camera.snapshot(picPath, this, ICameraP2P.PLAYMODE.LIVE);
  
  
  
```

  截屏操作回调

  ```java
  ···
  // 截屏操作回调
      @Override
      public void onSnapshotSuccessCallback() {
          Log.d(TAG, "onSnapshotSuccessCallback");
      }
  
      @Override
      public void onSnapshotFailCallback() {
          Log.d(TAG, "onSnapshotFailCallback");
      }
  ···
  ```



### 设置拾音器状态

```java
  int mute = 0;
              if (ICameraP2P.PLAYMODE.LIVE == playmode) {
                  mute = isPreviewMute == ICameraP2P.MUTE ? ICameraP2P.UNMUTE : ICameraP2P.MUTE;
              } else {
                  mute = isPlaybackMute == ICameraP2P.MUTE ? ICameraP2P.UNMUTE : ICameraP2P.MUTE;
              }
              camera.setMute(playmode, mute, this);
```

  回调方法

  ```java
  ···
    
      @Override
      public void onMuteOperateSuccess(ICameraP2P.PLAYMODE playmode, int isMute) {
          isPreviewMute = isMute;
          Log.d(TAG, "onPreviewMuteOperateSuccess mute" + isPreviewMute);
      }
  
      @Override
      public void onMuteOperateFail(ICameraP2P.PLAYMODE playmode) {
  
      }
  
  ···
  ```



### 开启对讲

> p2ptype=1的类型需要对讲操作前，首先要保证是否调用connectPlayback()操作成功。

```java
  try {
                      camera.startAudioTalk();
                  } catch (Exception e) {
                      e.printStackTrace();
                  }
```

  ```java
  ···
  	@Override
      public void onSpeakSuccessCallback(final boolean isTalking) {
          Log.d(TAG, "开启对讲成功");
          this.isSpeaking = isTalking;
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  if (isTalking)
                      startSpeakBtn.setText("对讲（开）");
                  else
                      startSpeakBtn.setText("对讲（关）");
              }
          });
      }
      
  	@Override
      public void onSpeakFailueCallback(int errorCode) {
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  startSpeakBtn.setText("对讲（关）");
              }
          });
      }
  
  ···
  ```





### 停止对讲

 ```java
   camera.stopAudioTalk();
 ```

  ```java
  ···
  	@Override
      public void onStopSpeakSuccessCallback() {
          Log.d(TAG, "开启对讲成功");
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  if (isTalking)
                      startSpeakBtn.setText("对讲（开）");
                  else
                      startSpeakBtn.setText("对讲（关）");
              }
          });
      }
      
      @Override
      public void onStopSpeakFailueCallback(int errorCode) {
  
      }
  ···
  ```

> 注：对讲和录制是互斥的，而且只有在预览时可以开启对讲。



### 分辨率改变

```java
  			camera.getVideoClarity();
              camera.setVideoClarity(ICameraP2P.HD);
```

  ```java
  ···
  @Override
      public void onDefinitionStatusCallback(boolean isQuery, int definition) {
          if (definition != null) {
              mDefinition = definition;
              runOnUiThread(new Runnable() {
                  @Override
                  public void run() {
                      enableHDBtn.setText(mDefinition == 4 ? "高清" : "标清");
                  }
              });
          }
      }
  ···
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

获取清晰度

```java
 camera.getVideoClarity();
```

回调方法和设置清晰度的回调方法一致



### 建立回放通道

仅p2pType=1的设备类型使用

  ```java
   camera.connectPlayback();
  ```

  ```java
  ···
  	@Override
      public void onChannel1StartSuccess() {
          Log.d(TAG, "playback 通道建立");
      }
      
   	@Override
      public void onChannelOtherStatus(int errorCode) {
          Log.d(TAG, "onChannelOtherStatus...errorCode " + errorCode);
      }
  
  	@Override
      public void onPlaybackEnterFail(String errorCode, String errorMsg) {
          Log.d(TAG, "playback 通道建立失败");
      }
  ···
  ```

  > 注意：获取如果要进行对讲操作，前提要建立playback channel通道



###  获取有回放视频记录的日期

  在开始回放前，需要获取到回放视频记录的信息。首先获取有回放视频记录的日期

  ```java
  camera.queryRecordDaysByMonth(2018, 7);
  ```

  ```java
  ···
     @Override
      public void onQueryPlaybackDataSuccessByMonth(int year, int month, Object days) {
          Log.d(TAG, "onQueryPlaybackDataSuccessByMonth ");
          String yearMonth = String.valueOf(year) + String.valueOf(month);
          mBackDataMonthCache.put(yearMonth, days);
      }
      
      @Override
      public void onQueryPlaybackDataFailureByMonth(int errorCode, String errorMsg) {
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  Toast.makeText(MainActivity.this, "获取日期数据失败。。。", Toast.LENGTH_SHORT).show();
              }
          });
      }
  ···    
  
  ```



### 获取某日的视频回放信息

获取到有用回放记录的日期后，根据日期获取当日的视频回放记录

```java
camera.queryRecordTimeSliceByDay(2018, 7, 8);
```

```java
···
	@Override
    public void onQueryPlaybackDataSuccessByDay(String yearmonthday, Object timePieceBeanList) {
        Log.d(TAG, "onQueryPlaybackDataSuccessByDay ");
        mBackDataDayCache.put(yearmonthday, timePieceBeanList);
    }

    @Override
    public void onQueryPlaybackDataFailureByDay(int errorCode, String errorMsg) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(MainActivity.this, "获取时间片段数据失败。。。", Toast.LENGTH_SHORT).show();
            }
        });
    }
···    
```

### 开启回放

  ```java
  playmode = ICamerapP2P.PLAYMODE.PLAYBACK;
              List<TimePieceBean> timePieceList = mBackDataDayCache.get("20180708");
              TimePieceBean defaultPlayTime = timePieceList.get(0);
              defaultPlayTime.setPlaytime(defaultPlayTime.getStarttime());
              camera.startPlayBack(defaultPlayTime.getStartTime(), defaultPlayTime.getEndTime(), defaultPlayTime.getStartTime());
       
  ```

  ```java
  ···
      @Override
      public void onPlaybackStartSuccess() {
          Log.d(TAG, "onPlaybackStartSuccess...");
      }
  
      @Override
      public void onPlaybackStartFail(String errorCode, String errorMsg) {
          Log.d(TAG, "onPlaybackStartFail...");
      }
  ···
  ```

 ### 暂停回放

  ```java
   camera.pausePlayback();
  ```

  ```java
  ···
      @Override
      public void onPlaybackPauseSuccess() {
          Log.d(TAG, "onPlaybackPauseSuccess...");
      }
  
      @Override
      public void onPlaybackPauseFail(String errorCode, String errorMsg) {
          Log.d(TAG, "onPlaybackPauseFail...");
      }
  ···
  ```

### 恢复回放

```java
camera.resumePlayback();
```

```java
···
    @Override
    public void onPlaybackResumeSuccess() {
        Log.d(TAG, "onPlaybackResumeSuccess...");
    }

    @Override
    public void onPlaybackResumeFail(String errorCode, String errorMsg) {
        Log.d(TAG, "onPlaybackResumeFail...");
    }
···
```

### 回放结束回调

```java
···   
	@Override
    public void onPlaybackEnd() {
        Log.d(TAG, "onPlaybackEnd ...");
    }
    
    @Override
    public void onPlaybackEndFail() {

    }
···    
```



###  获取硬解视频数据

> 注意：仅p2pType为1的设备可以进行硬解视频数据

```java
      @Override
          public void receiveFrameDataForMediaCodec(Camera camera, int avChannel, byte[] buf, int length,
                                                    int pFrmNo, byte[] pFrmInfoBuf, boolean isIframe, int codecId) {
              Log.e(TAG, "硬解获取码流 receiveFrameDataForMediaCodec ==== Camera =" + camera
                      + "  --avChannel =" + avChannel
                      + "  --buf =" + Arrays.toString(buf)
                      + "  --length =" + length
                      + "  --pFrmNo =" + pFrmNo
                      + "  --pFrmInfoBuf =" + Arrays.toString(pFrmInfoBuf)
                      + "  --isIframe =" + isIframe
                      + "  --codecId =" + codecId);
          }
```



### 获取视频流数据

> 注意：该方法仅为p2ptype=2的设备类型使用

  ```java

      @Override
      public void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera) {
          mVideoView.receiveFrameYUVData(sessionId, y, u, v, width, height, camera);
      }
  	
  ```



### 获取p2p状态

> 注意：该方法仅为p2ptype=2的设备类型使用

    ```java
     @Override
        public void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus) {
            Log.d(TAG, "sessionId " + sessionId + "sessionStatus " + sessionStatus);
        }
    ```
