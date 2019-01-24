# 摄像头功能具体方法调用及生命周期

1. 初始化

   Android摄像头sdk绑定播放器TuyaMonitorView，所以使用者首先创建TuyaMonitorView，并在初始化的时候给绑定到camera sdk中，同时要把OnDelegateCameraListener注入。如下所示：

   ```java
   private static final int ASPECT_RATIO_WIDTH = 9;
   private static final int ASPECT_RATIO_HEIGHT = 16;
   @Override
       protected void onCreate(Bundle savedInstanceState) {
           ···
           mVideoView = findViewById(camera_video_view);
           //设置播放器类型
           mIsRunSoft = getIntent().getBooleanExtra("isRunsoft", false);
           p2pType = getIntent().getIntExtra("p2pType", 1);
           mVideoView.createVideoView(p2pType, mIsRunSoft);
   		//播放器view最好宽高比设置16:9
          WindowManager windowManager = (WindowManager) this.getSystemService(WINDOW_SERVICE);
          int width = windowManager.getDefaultDisplay().getWidth();
          int height = width * ASPECT_RATIO_WIDTH / ASPECT_RATIO_HEIGHT;
          findViewById(R.id.camera_tutk_video_view_ll).setLayoutParams(new RelativeLayout.LayoutParams(width, height));
           ···
           initCamera();  //初始化
   
       }
   ```

   ```java
   /**
    * 初始化
    */    
   private void initCamera(){
           camera = TuyaSmartCameraFactory.generateTuyaSmartCamera(p2pType);
           camera.createDevice(p2pId, initStr, p2pKey, mIsRunSoft);
           camera.registorOnDelegateCameraListener(this);
           //是否开启软解/硬解，注意搭配各自的播放控件
           camera.generateCameraView(mVideoView.createdView());
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
       @Override
       public void onCreateDeviceSuccess() {
           Log.d(TAG, "onCreateDeviceSuccess");
       }
   
       @Override
       public void onCreateDeviceFail(int ret) {
           Log.d(TAG, "onCreateDeviceFail ret" + ret);
       }
   ```

2. connect 连线及connectChannel创建通道

   摄像头p2p连接操作及回调方法

   ```java
     camera.connect(p2pId, p2pWd,localKey);
   ```

     ```java
     public interface OnDelegateCameraListener {
     ···
          @Override
         public void connectFail(String errorCode, String errorMsg) {
             Log.d(TAG,"p2p通道构建失败 ： errorCode" + errorCode + "errorMsg " + errorMsg);
         }
         
         @Override
         public void onChannel0StartSuccess() {
             //live channel通道建立成功
             Log.d(TAG, "live 通道建立");
         }
         
         @Override
         public void onChannelOtherStatus(int errorCode) {
             //channel通道构建过程中的状态变化
             Log.d(TAG, "onChannelOtherStatus...errorCode " + errorCode);
         }
     ···    
     }
     ```

3. startPreview 开始播放影像

   摄像头播放影像操作及回调方法

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
             Log.d(TAG, "获取到影像iFrame"); //可以作为影像展示的判断
         }
     
     ···    
     }
     ```

4. stopPreview停止播放影像

   停止摄像头播放影像操作

   ```java
     camera.stopPreview();
   ```

5. disconnect断开连线

   ```java
     camera.disconnect();
   ```

6. destroy销毁对象

   ```java
      camera.onDestroy();
   ```

>  以上方法调用构成摄像头live的生命周期