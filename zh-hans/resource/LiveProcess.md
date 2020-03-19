# 视频直播

视频直播需要获取云端服务器信息，接着创建 `ICameraP2P` 对象，然后进行 P2P 连接后，就可以播放实时视频，截图、录制视频和实时对讲数据传输。



## 示例代码



### 主要链路代码



#### 1. 初始化

`ICameraP2P` 需要绑定 `Monitor`，使用者要创建 `ICameraP2P` 和 `Monitor` ,同时也需要注册`OnP2PCameraListener` 。如下所示：

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
   
           //播放器view最好宽高比设置16:9
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
       		mCameraP2P.generateCameraView(mVideoView); //绑定 monitor
           getApi(); //获取设备信息
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



#### 2. 获取设备信息

通过调用云端接口，获取设备配置相关信息。

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



#### 3. P2P 连接

在开始视频播放之前，需要先连接 P2P 通道。P2P 状态需要使用者自己维护，SDK 只负责下发指令和接收摄像机响应结果。

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

#### 4. 开启实时播放视频

P2P连接成功之后，就能进行实时视频播放了。

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

> 注意：startPreview 成功回调之后，onReceiveFrameYUVData 回调会开始接收视频数据，并抛给业务层。



#### 5. 停止实时播放视频

停止播放摄像机实时视频操作。

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



#### 6. 断开P2P连接

disconnect 断开连线，离开页面的时候可以选择断开 p2p 连接。

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



#### 7. 销毁ICameraP2P对象

destroy 销毁对象，不再使用 camera 功能的时候，一定要调用 destroy

```java
TuyaSmartCameraP2PFactory.onDestroyTuyaSmartCamera();   
```

>  以上方法调用构成摄像头live的生命周期



## 流程图

<img src="./images/live_process_image.jpg" style="zoom:60%;" />

## 实时视频开启成功后，可调用信令



### 视频录制

视频录制功能可以把实时视频的影像文件以 mp4 格式存储到手机 SD 卡上。



#### 开启视频录制

调用 startRecordLocalMp4 方法。

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



#### 停止视频录制

调用 stopRecordLocalMp4 方法。

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



#### 视频截图

截取实时视频的影像图片存储到手机 SD 卡上。

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



#### 设置拾音器状态

设置设备传过来的声音开关

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



#### 开启对讲

打开手机声音传输给摄像机操作。

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



 #### 停止对讲

关闭手机声音传输给摄像机操作。

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



#### 获取清晰度

获取摄像机传过来的影像清晰度。

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

#### 设置清晰度

设置摄像机播放的影像清晰度。

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

#### 清晰度模式

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




