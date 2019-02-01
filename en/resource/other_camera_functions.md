# Other methods invocation



### Turn on video recording


There are 2 methods to use, both of them are blocking methods. Suggest to manage in the sub thread.

-  the recording method without audio data

```java
	String mRecordMp4Path = null;
	if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
	    String picPath = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
		String fileName = System.currentTimeMillis() + ".mp4";
	}
	boolean isRecording = false;
	try {
		isRecording = camera.startRecordingWithoutAudio(mRecordMp4Path, fileName, MainActivity.this);  //		isRecording = true, video recording call successfully
	} catch (Exception e) {
	  e.printStackTrace();
	}
	if (isRecording) {
	  Toast.makeText(MainActivity.this, "start record...", Toast.LENGTH_SHORT).show();
	} else {
	  Toast.makeText(MainActivity.this, "record fail", Toast.LENGTH_SHORT).show();
	}       
  ```

- the recording method with audio data

```java
	    String mRecordMp4Path = null;
	    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
	       String picPath = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
	    String fileName = System.currentTimeMillis() + ".mp4";
	    }
        boolean isRecording = false;
        try {
            isRecording = camera.startRecordLocalMp4(mRecordMp4Path, fileName, MainActivity.this);  //isRecording = true, means video recording call successfully
        } catch (Exception e) {
            e.printStackTrace();
        }
        if (isRecording) {
            Toast.makeText(MainActivity.this, "start record..", Toast.LENGTH_SHORT).show();
        } else {
            Toast.makeText(MainActivity.this, "record fail", Toast.LENGTH_SHORT).show();
        }     
```

> tip: video recording requires to write memory card permission



### Stop video recording

```java
  boolean isStopRecord = false;
  try {
      isStopRecord = camera.stopRecordLocalMp4();
  } catch (Exception e) {
      e.printStackTrace();
  }
  if (isStopRecord) {
      Toast.makeText(MainActivity.this, "record over...", Toast.LENGTH_SHORT).show();
  } else {
      Toast.makeText(MainActivity.this, "stop record fail...", Toast.LENGTH_SHORT).show();
  }
```

> tip: video will be saved in custom routine once it recorded successfully.



### video screenshot

video screen shot  needs to be divided by live mode and playback mode

```java
		String picPath = null;
       if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
              picPath = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/" + 				System.currentTimeMillis() + ".png";
      		}
      camera.snapshot(picPath, this, ICameraP2P.PLAYMODE.LIVE);
  
```

   screenshot operation callback

  ```java
  ···
  //  screenshot operation callback
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



### set microphone status

```java
  int mute = 0;
  if (ICameraP2P.PLAYMODE.LIVE == playmode) {
      mute = isPreviewMute == ICameraP2P.MUTE ? ICameraP2P.UNMUTE : ICameraP2P.MUTE;
  } else {
      mute = isPlaybackMute == ICameraP2P.MUTE ? ICameraP2P.UNMUTE : ICameraP2P.MUTE;
  }
  camera.setMute(playmode, mute, this);
```

  callback method

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



### open intercom

> p2ptype=1 type requires to confirm whether connectPlayback() call successfully before the intercom operation

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

          this.isSpeaking = isTalking;
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  if (isTalking)
                      startSpeakBtn.setText("talk（open）");
                  else
                      startSpeakBtn.setText("talk（close）");
              }
          });
      }
      
  	@Override
    public void onSpeakFailueCallback(int errorCode) {
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  startSpeakBtn.setText("talk（close）");
              }
          });
      }
  
  ···
  ```





### Stop intercom

 ```java
   camera.stopAudioTalk();
 ```

  ```java
  ···
  	@Override
      public void onStopSpeakSuccessCallback() {
          runOnUiThread(new Runnable() {
              @Override
              public void run() {
                  if (isTalking)
                      startSpeakBtn.setText("talk（open）");
                  else
                      startSpeakBtn.setText("talk（close）");
              }
          });
      }
      
      @Override
      public void onStopSpeakFailueCallback(int errorCode) {
  
      }
  ···
  ```

> tip: intercom and recording are exclusive, intercom can be opened only during live view



### resolution change

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
                  enableHDBtn.setText(mDefinition == 4 ? "HD" : "SD");
              }
          });
      }
  }
  ···
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
  get definition

```java
 camera.getVideoClarity();
```


the method of callback and set definition is the same



### Set up playback channel


only for p2pType=1 device type use

  ```java
   camera.connectPlayback();
  ```

  ```java
  ···
  	@Override
      public void onChannel1StartSuccess() {
          Log.d(TAG, "playback channel success");
      }
      
   	@Override
      public void onChannelOtherStatus(int errorCode) {
          Log.d(TAG, "onChannelOtherStatus...errorCode " + errorCode);
      }
  
  	@Override
      public void onPlaybackEnterFail(String errorCode, String errorMsg) {
          Log.d(TAG, "playback channel fail");
      }
  ···
  ```

  > tips: If you want to talk, you need to build a playback channel.



###  Get the dates that have playback video recording info

Need to get playback video recording info before playback. First, get the dates that have playback video recording info.

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
                  Toast.makeText(MainActivity.this, "get date fail。。。", Toast.LENGTH_SHORT).show();
              }
          });
      }
  ···    
  
  ```



### Get the video playback info of someday


After getting the dates that have playback recording, query playback data by day.

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
                Toast.makeText(MainActivity.this, "get timepieces`s data fail。。。", Toast.LENGTH_SHORT).show();
            }
        });
    }
···    
```

### Start playback

  ```java
	playmode = ICamerapP2P.PLAYMODE.PLAYBACK;
	List<TimePieceBean> timePieceList = mBackDataDayCache.get("20180708");
	TimePieceBean defaultPlayTime = timePieceList.get(0);
	defaultPlayTime.setPlaytime(defaultPlayTime.getStarttime());
	camera.startPlayBack(defaultPlayTime.getStartTime(), defaultPlayTime.getEndTime(), 	defaultPlayTime.getStartTime());
       
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

 ### Pause playback

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

### Resume playback

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

### Playback end callback

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



###   get frame data for media code

> tip: only for devices that p2pType =1 

```java
      @Override
      public void receiveFrameDataForMediaCodec(Camera camera, int avChannel, byte[] buf, int length,
                                                    int pFrmNo, byte[] pFrmInfoBuf, boolean isIframe, int codecId) {
              Log.e(TAG, "receiveFrameDataForMediaCodec ==== Camera =" + camera
                      + "  --avChannel =" + avChannel
                      + "  --buf =" + Arrays.toString(buf)
                      + "  --length =" + length
                      + "  --pFrmNo =" + pFrmNo
                      + "  --pFrmInfoBuf =" + Arrays.toString(pFrmInfoBuf)
                      + "  --isIframe =" + isIframe
                      + "  --codecId =" + codecId);
          }
```



###  Get streaming video data


> tip: only for devices that p2pType =2

  ```java

      @Override
      public void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, long timestamp, Object camera) {
          mVideoView.receiveFrameYUVData(sessionId, y, u, v, width, height, camera);
      }
  	
  ```



###  Get p2p status

> tip: only for devices that p2pType =2

```java

@Override
public void onSessionStatusChanged(Object camera, int sessionId, int sessionStatus) {
Log.d(TAG, "sessionId " + sessionId + "sessionStatus " + sessionStatus);
}

```