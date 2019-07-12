# Camera specific function call method and life cycle

### 1.initialize

Because android camera sdk is bound with TuyaMonitorView, the user need to create TuyaMonitorView, and bound it with TuyaSmartCameraSDK when you initialize it, in the meantime registor the OnP2PCameraListener. please check below：

```java
	private static final int ASPECT_RATIO_WIDTH = 9;
	private static final int ASPECT_RATIO_HEIGHT = 16;
@Override
    protected void onCreate(Bundle savedInstanceState) {
        ···
        initView();
        initData();
        initListener();
        ···

    }
```

 
```java      
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
        getApi(); //get ipc config 
    }

```
   
```java

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
    // playmode
	public static enum PLAYMODE {
        LIVE(0),     // live
        PLAYBACK(1); // playback

        int value;

        private PLAYMODE(int value) {
            this.value = value;
        }

        public int getValue() {
            return this.value;
        }
    }
```
 call initialization

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

### 2.connect 

   camera p2p operate and call method

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

### 3.startPreview 

   camera play video operation and call method

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

> Note：After the successful callback of startPreview, the onReceive Frame YUVData callback starts to receive video data and throw it to the business layer.

### 4.stopPreview

   operation of stop preview

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
  
### 5.disconnect

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

### 6.destroy
you must destroy instances,if you no longer use camera

```java
  TuyaSmartCameraP2PFactory.onDestroyTuyaSmartCamera();   
```

>  upon is the camera life cycle