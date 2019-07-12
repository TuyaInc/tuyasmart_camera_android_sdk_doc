# 摄像头功能具体方法调用及生命周期

1. 初始化

   Android摄像头sdk绑定播放器Monitor，所以使用者首先创建Monitor，并在初始化的时候给绑定到camera sdk中，同时要把OnP2PCameraListener注入。如下所示：

   ```java
   private static final int ASPECT_RATIO_WIDTH = 9;
   private static final int ASPECT_RATIO_HEIGHT = 16;
   @Override
       protected void onCreate(Bundle savedInstanceState) {
           ···
        initView();
        initData();
        initListener();
   
       }
       
 ```
 
 ```java      
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
 ```
 
 ```java   
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
        getApi(); //获取设备信息
    }

   ```
   
   ```
   	private void getApi() {
        Map postData = new HashMap();
        postData.put("devId", devId);
        TuyaHomeSdk.getRequestInstance().requestWithApiName("tuya.m.ipc.config.get", "2.0",
                postData, new IRequestCallback() {
                    @Override
                    public void onSuccess(Object o) {
                        infoBean = JSONObject.parseObject(o.toString(), CameraInfoBean.class);
                        Log.d("onSuccess", o.toString());
                        mP2p3Id = infoBean.getId(); // not support yet
                        p2pType = infoBean.getP2pSpecifiedType();
                        p2pId = infoBean.getP2pId().split(",")[0];
                        p2pWd = infoBean.getPassword();
                        mInitStr = infoBean.getP2pConfig().getInitStr();
                        mP2pKey = infoBean.getP2pConfig().getP2pKey();
                        mInitStr += ":" + mP2pKey;
                        if (null != infoBean.getP2pConfig().getIces()) {
                            token = infoBean.getP2pConfig().getIces().toString();
                        }
                        initCameraView();
                    }

                    @Override
                    public void onFailure(String s, String s1) {
                        ToastUtil.shortToast(CameraPanelActivity.this, "get cameraInfo failed");
                    }
                });
    }
   ```

   ```java
       //播放模式
   	public static enum PLAYMODE {
           LIVE(0),     //直播
           PLAYBACK(1); //回放
   
           int value;
   
           private PLAYMODE(int value) {
               this.value = value;
           }
   
           public int getValue() {
               return this.value;
           }
       }
   ```

   初始化回调

   ```java
       mCameraP2P.createDevice(new OperationDelegateCallBack() {
                @Override
                public void onSuccess(int sessionId, int requestId, String data) {
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_CREATE_DEVICE, ARG1_OPERATE_SUCCESS));
                }

                @Override
                public void onFailure(int sessionId, int requestId, int errCode) {
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_CREATE_DEVICE, ARG1_OPERATE_FAIL));
                }
            }, p2pType, devId, p2pId, mInitStr, "");
   ```

2. connect 连线及connectChannel创建通道

   摄像头p2p连接操作及回调方法

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
        }, p2pId, p2pWd, localKey, token);
   ```

3. startPreview 开始播放影像

   摄像头播放影像操作及回调方法

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
	> 注意：startPreview成功回调之后，onReceiveFrameYUVData回调会开始接收视频数据，并抛给业务层。
	
4. stopPreview停止播放影像

   停止摄像头播放影像操作

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

5. disconnect断开连线,离开页面的时候可以选择断开p2p连接

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

6. destroy销毁对象，不再使用camera功能的时候，一定要调用destroy

   ```java
      TuyaSmartCameraP2PFactory.onDestroyTuyaSmartCamera();   
   ```

>  以上方法调用构成摄像头live的生命周期