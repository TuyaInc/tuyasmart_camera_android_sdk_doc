# Camera specific function call method and life cycle

### 1.initialize

Because android camera sdk is bound with TuyaMonitorView, the user need to create TuyaMonitorView, and bound it with TuyaSmartCameraSDK when you initialize it, in the meantime registor the OnDelegateCameraListener. please check below：

```java
	private static final int ASPECT_RATIO_WIDTH = 9;
	private static final int ASPECT_RATIO_HEIGHT = 16;
@Override
    protected void onCreate(Bundle savedInstanceState) {
        ···
        mVideoView = findViewById(camera_video_view);
        // set player type
        mIsRunSoft = getIntent().getBooleanExtra("isRunsoft", false);
        p2pType = getIntent().getIntExtra("p2pType", 1);
        mVideoView.createVideoView(p2pType, mIsRunSoft);
		// player view set suggest to be 16:9
       WindowManager windowManager = (WindowManager) this.getSystemService(WINDOW_SERVICE);
       int width = windowManager.getDefaultDisplay().getWidth();
       int height = width * ASPECT_RATIO_WIDTH / ASPECT_RATIO_HEIGHT;
       findViewById(R.id.camera_tutk_video_view_ll).setLayoutParams(new RelativeLayout.LayoutParams(width, height));
        ···
       initCamera();  // initialization

    }
```

```java
/**
 *  initialization
 */    
private void initCamera(){
        camera = TuyaSmartCameraFactory.generateTuyaSmartCamera(p2pType);
        camera.createDevice(p2pId, mInitString, mIsRunSoft);
        camera.registorOnDelegateCameraListener(this);
        camera.generateCameraView(mVideoView.createdView());
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
    @Override
    public void onCreateDeviceSuccess() {
        Log.d(TAG, "onCreateDeviceSuccess");
    }

    @Override
    public void onCreateDeviceFail(int ret) {
        Log.d(TAG, "onCreateDeviceFail ret" + ret);
    }
    
```

### 2.connect 

   camera p2p operate and call method

  ```java
  	camera.connect(p2pId, p2pWd,localKey);
  ```

  ```java
  	public interface OnDelegateCameraListener {
  	···
      @Override
      public void connectFail(String errorCode, String errorMsg) {
          Log.d(TAG,"p2p channel fail ： errorCode" + errorCode + "errorMsg " + errorMsg);
      }
      
      @Override
      public void onChannel0StartSuccess() {
          //live channel success
          Log.d(TAG, "live success");
      }
      
      @Override
      public void onChannelOtherStatus(int errorCode) {
          //channel state
          Log.d(TAG, "onChannelOtherStatus...errorCode " + errorCode);
      }
  ···    
  }
  ```

### 3.startPreview 

   camera play video operation and call method

  ```java
  playmode = ICamerapP2P.PLAYMODE.LIVE;
  camera.startPreview();
  ```

  ```java
  public interface OnDelegateCameraListener {
	  ···
	  
	@Override
	public void onPreviewSuccess() {
	    Log.d(TAG, "onPreviewSuccess");
	}
	
	@Override
	public void onPreviewFail(int errorCode) {
	    Log.d(TAG, "onPreviewFail errorCode" + errorCode);
	}
	    
	@Override
	public void onreceiveFrameDataCallback() {
	  Log.d(TAG, "get iFrame"); 
	}
  
  ···    
  }
  ```

### 4.stopPreview

   operation of stop preview

  ```java
  camera.stopPreview();
  
  ```
  
### 5.disconnect

   ```java
  camera.disconnect();
   ```

### 6.destroy

 ```java
 camera.onDestroy();
 ```

>  upon is the camera life cycle