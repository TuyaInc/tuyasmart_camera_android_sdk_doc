

# Memory card playback

Tuya IPC camera supports SD card recording function. After the smart camera inserts the memory card, you can view the information and status of the memory card, and set the recording switch and mode. For details, refer to the [memory card management function](./camera_device_points.md#Memory card and local video recording management).

The memory card playback is based on the **Tuya Smart Camera Android** SDK's cloud server information, then creating an ICameraP2P object, and then making a P2P connection, you can play the video recorded by the camera's SD card.





## Sample code



### Main link code



#### 1. init

`ICameraP2P` needs to be bound to `IMonitorView`, users need to create `ICameraP2P` and `TuyaCameraView`, then use `TuyaCameraView` to construct `IMonitorView`, and also need to register `OnP2PCameraListener` and `CreateVideoViewCallback`.

```java
// construct ICameraP2P
ICameraP2P mCameraP2P = TuyaSmartCameraP2PFactory.generateTuyaSmartCamera(sdkProvider);

// construct TuyaCameraView
TuyaCameraView mVideoView = findViewById(R.id.camera_video_view);

// set callback for TuyaCameraView，the type is TuyaCameraView.CreateVideoViewCallback
mVideoView.setCameraViewCallback(this);

// use TuyaCameraView to construct IMonitorView
mVideoView.createVideoView(sdkProvider);

...

// CreateVideoViewCallback callback when IMonitorView construction is complete
@Override
public void onCreated(Object view) {
  if (null != mCameraP2P){

    // Bind IMonitorView to ICameraP2P
    mCameraP2P.generateCameraView(view);
  }
}
```



#### 2. Get device information and create device

Get device configuration related information by calling the cloud interface.

```java
// get device information
private void getApi() {
    mSmartCameraP2P = new TuyaSmartCameraP2P();
    mSmartCameraP2P.requestCameraInfo(devId, new ICameraConfig() {
        @Override
        public void onFailure(BusinessResponse var1, ConfigCameraBean var2, String var3) {
            ToastUtil.shortToast(CameraPanelActivity.this, "get cameraInfo failed");
        }

        @Override
        public void onSuccess(BusinessResponse var1, ConfigCameraBean var2, String var3) {
            p2pWd = var2.getPassword();
            p2pId = var2.getP2pId();
            // create device
            initCameraView(var2);
        }
    });
}

private void initCameraView(ConfigCameraBean bean) {
    mCameraP2P.createDevice(new OperationDelegateCallBack() {
        @Override
        public void onSuccess(int sessionId, int requestId, String data) {
            mHandler.sendMessage(MessageUtil.getMessage(MSG_CREATE_DEVICE, ARG1_OPERATE_SUCCESS));
        }

        @Override
        public void onFailure(int sessionId, int requestId, int errCode) {
            mHandler.sendMessage(MessageUtil.getMessage(MSG_CREATE_DEVICE, ARG1_OPERATE_FAIL));
        }
    },bean);
}
```



#### 3. P2P Connect

Before starting video playback, you need to connect the P2P channel. The P2P status needs to be maintained by the user. The SDK is only responsible for issuing instructions and receiving camera response results.

```java
mCameraP2P.connect(new OperationDelegateCallBack() {
            @Override
            public void onSuccess(int sessionId, int requestId, String data) {
                mHandler.sendMessage(MessageUtil.getMessage(MSG_CONNECT, ARG1_OPERATE_SUCCESS));
            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {
                mHandler.sendMessage(MessageUtil.getMessage(MSG_CONNECT, ARG1_OPERATE_FAIL, errCode));
            }
});
```



#### 4. Get date with playback video record

Call queryRecordDaysByMonth method.

```java
int year = Integer.parseInt(substring[0]);
int mouth = Integer.parseInt(substring[1]);
queryDay = Integer.parseInt(substring[2]);
mCameraP2P.queryRecordDaysByMonth(year, mouth, new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
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

> Before starting playback, you need to get the information of the playback video record. First get the date with the playback video record.



#### 5. Get video playback information for a certain day

```java
int year = Integer.parseInt(substring[0]);
int mouth = Integer.parseInt(substring[1]);
int day = Integer.parseInt(substring[2]);
mCameraP2P.queryRecordTimeSliceByDay(year, mouth, day, new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {		
    parsePlaybackData(data);
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
    mHandler.sendEmptyMessage(MSG_DATA_DATE_BY_DAY_FAIL);
  }
});
```

> After getting the date of useful playback record, get the video playback record of the day according to the date.

#### 6. Start Play back

```java
mCameraP2P.startPlayBack(timePieceBean.getStartTime(),
                         timePieceBean.getEndTime(),
                         timePieceBean.getStartTime(), new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data){
      isPlayback = true;
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode){
      isPlayback = false;
    }
  }, new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data){
      isPlayback = false;
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode){
      isPlayback = false;
    }
});
```



#### 7. Pause playback

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



#### 8. Resume playback

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



#### 9. Stop playback

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



#### 10. Disconnect P2P

you can choose to disconnect p2p connection when leaving the page.

```java
mCameraP2P.disconnect(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {

  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
```



#### 11. Destroy Camera

Destroy the Camera, when you no longer use the camera function, you must call destroy.

```java
TuyaSmartCameraP2PFactory.onDestroyTuyaSmartCamera();   
```





# Flow chart



<img src="./images/playback_process.jpg" style="zoom:50%;" />



## After the playback is successfully started, the signaling can be called



### Video recording

Call the startRecordLocalMp4 method.

**Sample code**

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

> Note: Recording video requires write card permission



### Stop video recording

Call the stopRecordLocalMp4 method.

**Sample code**

```java
mCameraP2P.stopRecordLocalMp4(new OperationDelegateCallBack() {
                @Override
                public void onSuccess(int sessionId, int requestId, String data) {
                    isRecording = false;
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_VIDEO_RECORD_OVER, ARG1_OPERATE_SUCCESS, data));
                }

                @Override
                public void onFailure(int sessionId, int requestId, int errCode) {
                    isRecording = false;
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_VIDEO_RECORD_OVER, ARG1_OPERATE_FAIL));
                }
            });
```



### Video screenshot

Capture live video images and store them on the SD card of the mobile phone.

**Sample code**

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
                mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_SUCCESS, data));
            }

            @Override
            public void onFailure(int sessionId, int requestId, int errCode) {
                mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_FAIL));
            }
        });
```



### Set Mute Switch

Set the sound switch from the device

**Sample code**

```java
int mute;
mute = previewMute == ICameraP2P.MUTE ? ICameraP2P.UNMUTE : ICameraP2P.MUTE;
mCameraP2P.setMute(ICameraP2P.PLAYMODE.LIVE, mute, new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    previewMute = Integer.valueOf(data);
    mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_SUCCESS));
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
    mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_FAIL));
  }
});
```

