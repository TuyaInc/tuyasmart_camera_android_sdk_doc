# Live Video



## Introduction

For live video, you need to obtain the cloud server information, then create an `ICameraP2P` object, and then make a P2P connection to play the live video.Screenshots, recorded videos, and real-time intercom data transfers.



## Sample Code



### Main link code



#### 1. init

`ICameraP2P` needs to be bound to `Monitor`, users need to create `ICameraP2P` and `Monitor`, and also need to register `OnP2PCameraListener`.

```java
private static final int ASPECT_RATIO_WIDTH = 9;
   private static final int ASPECT_RATIO_HEIGHT = 16;
   @Override
       protected void onCreate(Bundle savedInstanceState) {
           ···
        initView();
        initData();
        initListener();
   		...
       }

		private void initView() {
           ...
           mVideoView = findViewById(R.id.camera_video_view);
           ...
           WindowManager windowManager = (WindowManager) this.getSystemService(WINDOW_SERVICE);
           int width = windowManager.getDefaultDisplay().getWidth();
           int height = width * ASPECT_RATIO_WIDTH / ASPECT_RATIO_HEIGHT;
           RelativeLayout.LayoutParams layoutParams = new RelativeLayout.LayoutParams(width, height);
           layoutParams.addRule(RelativeLayout.BELOW, R.id.toolbar_view);
           findViewById(R.id.camera_video_view_Rl).setLayoutParams(layoutParams);
   
   }

private void initData() {
           localKey = getIntent().getStringExtra(INTENT_LOCALKEY);
           devId = getIntent().getStringExtra(INTENT_DEVID);
           sdkProvider = getIntent().getIntExtra(INTENT_SDK_POROVIDER, -1);
           mIsRunSoft = getIntent().getBooleanExtra("isRunsoft", true);
           if (null != TuyaHomeSdk.getUserInstance().getUser()) {
               mlocalId = TuyaHomeSdk.getUserInstance().getUser().getUid();
           }
           mCameraP2P = TuyaSmartCameraP2PFactory.generateTuyaSmartCamera(sdkProvider);
           mDeviceControl = TuyaCameraDeviceControlSDK.getCameraDeviceInstance(devId);
       		mCameraP2P.generateCameraView(mVideoView); 
           getApi();
       }

private void initCameraView() {
           mCameraP2P.createDevice(new OperationDelegateCallBack() {
               @Override
               public void onSuccess(int sessionId, int requestId, String data) {
                   mHandler.sendMessage(MessageUtil.getMessage(MSG_CREATE_DEVICE, ARG1_OPERATE_SUCCESS));
               }
   
               @Override
               public void onFailure(int sessionId, int requestId, int errCode) {
                   mHandler.sendMessage(MessageUtil.getMessage(MSG_CREATE_DEVICE, ARG1_OPERATE_FAIL));
               }
           },configCameraBean);
       }
...
```



#### 2. Get device information

Get device configuration related information by calling the cloud interface.

```java
private void getApi() {
        Map postData = new HashMap();
        postData.put("devId", devId);
        CameraBusiness cameraBusiness = new CameraBusiness();
        cameraBusiness.requestCameraInfo(devId, new Business.ResultListener<CameraInfoBean>() {
            @Override
            public void onFailure(BusinessResponse businessResponse, CameraInfoBean cameraInfoBean, String s) {
                ToastUtil.shortToast(CameraPanelActivity.this, "get cameraInfo failed");
            }

            @Override
            public void onSuccess(BusinessResponse businessResponse, CameraInfoBean cameraInfoBean, String s) {
                configCameraBean = new ConfigCameraBean();

                infoBean = cameraInfoBean;

                mP2p3Id = infoBean.getId();
                p2pType = infoBean.getP2pSpecifiedType();
                p2pId = infoBean.getP2pId().split(",")[0];
                p2pWd = infoBean.getPassword();
                mInitStr = infoBean.getP2pConfig().getInitStr();
                mP2pKey = infoBean.getP2pConfig().getP2pKey();
                mInitStr += ":" + mP2pKey;
                if (null != infoBean.getP2pConfig().getIces()) {
                    token = infoBean.getP2pConfig().getIces().toString();
                }
                configCameraBean.setDevId(devId);
                configCameraBean.setLocalKey(localKey);
                configCameraBean.setInitString(mInitStr);
                configCameraBean.setToken(token);
                configCameraBean.setP2pType(p2pType);
                configCameraBean.setLocalId(mlocalId);
                configCameraBean.setPassword(p2pWd);
                if (P2P_2 == p2pType) {
                    configCameraBean.setP2pId(p2pId);
                }else if (P2P_4 == p2pType){
                    configCameraBean.setP2pId(mP2p3Id);
                }
                initCameraView();
            }
        });
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



#### 4. start Live Video

After the P2P connection is successful, Live video  is possible.

```java
mCameraP2P.startPreview(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    Log.d(TAG, "start preview onSuccess");
    isPlay = true;
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
    Log.d(TAG, "start preview onFailure");
    isPlay = false;
  }
});
```

> Note: After the startPreview callback is successful, the onReceiveFrameYUVData callback will start receiving video data and throw it to the business layer.

  

#### 5. stop Live Video

Stop playing the camera live video operation.

```java
camera.stopPreview(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {

  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
```



#### 6. Disconnect P2P 

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



#### 7. Destroy ICameraP2P

Destroy the object, when you no longer use the camera function, you must call destroy

 ```java
TuyaSmartCameraP2PFactory.onDestroyTuyaSmartCamera();   
 ```

> The above method calls constitute the life cycle of the camera live



## Flow chart



![](./images/live_process_image.jpg)



## After Live is successfully opened, you can call signaling

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



### Start Audio Talk

Turn on the phone to transmit the sound to the camera for operation.

**Sample code**

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



### Stop Audio Talk

Turn off the phone sound transmission to the camera operation.

**Sample code**

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

> Note: Intercom and recording are mutually exclusive, and intercom can only be turned on during preview.



### Get Video Clarity

Get the sharpness of the image transmitted from the camera.

**Sample code**

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

> Note: After the preview screen comes out, call this function



### Set Video Clarity

Set the sharpness of the image played by the camera.

**Sample code**

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



#### Clarity model

```java
public interface ICameraP2P<T> {

      int HD = 4;
      int STANDEND = 2;
      ...
          
      }


```

