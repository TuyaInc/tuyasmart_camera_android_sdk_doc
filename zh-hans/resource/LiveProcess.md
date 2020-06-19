# 摄像机功能

Camera SDK 提供智能摄像机的实时视频播放，设备存储卡录像播放，对当前正在播放的视频截图、录制视频， 与摄像机实时通话等基础能力，并提供视频帧解码后的 YUV 数据，开发者可以对视频数据进行二次处理。



## 视频直播

视频直播需要获取云端服务器信息，接着创建 `ICameraP2P` 对象，然后进行 P2P 连接后，就可以播放实时视频，截图、录制视频和实时对讲数据传输。

### 初始化

`ICameraP2P` 需要绑定渲染视图，使用者要创建 `ICameraP2P` 和 `TuyaCameraView` ，再使用 `TuyaCameraView` 构造渲染视图，同时也需要注册 `OnP2PCameraListener`、`CreateVideoViewCallback` 。

#### 1. 创建 ICameraP2P 对象

**接口说明**

```java
public static ICameraP2P generateTuyaSmartCamera(int sdkprovider);
```

**参数说明**

|     参数     |   说明   |
| ------------ | ------- |
| sdkprovider | 设备 P2P 类型，从 DeviceBean 中获取，详见[P2P 类型](./home_device.md#摄像机) |

**示例代码**

```java
ICameraP2P mCameraP2P = TuyaSmartCameraP2PFactory.generateTuyaSmartCamera(sdkProvider);
```

#### 2. 为渲染视图容器设置回调

**接口说明**

```java
public void setCameraViewCallback(CreateVideoViewCallback callback)
```

**参数说明**

|   参数    |   说明   |
| -------- | ------- |
| callback | 回调接口 |

**示例代码**

```java
TuyaCameraView mVideoView = findViewById(R.id.camera_video_view);
mVideoView.setCameraViewCallback(new TuyaCameraView.CreateVideoViewCallback() {
    @Override
    public void onCreated(Object o) {
        //渲染视图构造完成时回调
    }

    @Override
    public void videoViewClick() {

    }

    @Override
    public void startCameraMove(PTZDirection ptzDirection) {

    }

    @Override
    public void onActionUP() {

    }
});
```

#### 3. 构造渲染视图

**接口说明**

```java
public void createVideoView(int provider);
```

**参数说明**

|   参数    |   说明   |
| -------- | ------- |
| provider | 设备类型 |

**示例代码**

```
TuyaCameraView mVideoView = findViewById(R.id.camera_video_view);
mVideoView.createVideoView(sdkProvider);
```

**接口说明**

获取视频渲染视图

> 需要注意如果未构造视频渲染视图会返回 null

```java
public Object createdView();
```

**示例代码**

```java
TuyaCameraView mVideoView = findViewById(R.id.camera_video_view);
mVideoView.createdView()
```

#### 4. 为 ICameraP2P 绑定渲染视图

**接口说明**

```java
void generateCameraView(T view);
```

**示例代码**

```java
mCameraP2P.generateCameraView(mVideoView.createdView());
```

#### 链路

**示例代码**

```java
// 1. 创建 ICameraP2P
ICameraP2P mCameraP2P = TuyaSmartCameraP2PFactory.generateTuyaSmartCamera(sdkProvider);
// 2. 为渲染视图容器设置回调
TuyaCameraView mVideoView = findViewById(R.id.camera_video_view);
mVideoView.setCameraViewCallback(new TuyaCameraView.CreateVideoViewCallback() {
    @Override
    public void onCreated(Object o) {
        //4. 渲染视图构造完成后，为 ICameraP2P 绑定渲染视图
        mCameraP2P.generateCameraView(mVideoView.createdView());
    }

    @Override
    public void videoViewClick() {

    }

    @Override
    public void startCameraMove(PTZDirection ptzDirection) {

    }

    @Override
    public void onActionUP() {

    }
});
// 3. 构造渲染视图 IMonitorView
mVideoView.createVideoView(sdkProvider);

```

### 获取设备信息

通过调用云端接口，获取设备配置相关信息。

**接口说明**

```java
void requestCameraInfo(String devId, ICameraConfig callback);
```

**参数说明**

|   参数    |  说明   |
| -------- | ------- |
| devId    | 设备 id |
| callback | 结果回调 |

**示例代码**

```java
TuyaSmartCameraP2P mSmartCameraP2P = new TuyaSmartCameraP2P();
mSmartCameraP2P.requestCameraInfo(devId, new ICameraConfig() {
    @Override
    public void onSuccess(BusinessResponse var1, ConfigCameraBean var2, String var3) {
        //获取信息成功
    }

    @Override
    public void onFailure(BusinessResponse var1, ConfigCameraBean var2, String var3) {
        //获取信息失败
    }
}
```

### 创建设备

需要在获取设备配置信息之后创建设备才能进行 P2P 连接

**接口说明**

```java
void createDevice(OperationDelegateCallBack callback, ConfigCameraBean bean);
```

**参数说明**

|   参数    |           说明            |
| -------- | ------------------------- |
| callback | 结果回调                   |
| bean     | 配置信息，获取设备信息时会返回 |

**示例代码**

```java
mCameraP2P.createDevice(new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data) {
        //创建成功
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode) {
        //创建失败
    }
}, bean);
```

### P2P 连接

在开始视频播放之前，需要先连接 P2P 通道。P2P 状态需要使用者自己维护，SDK 只负责下发指令和接收摄像机响应结果。

**接口说明**

开始连接 P2P 通道

```java
void connect(OperationDelegateCallBack callBack);
```

断开 P2P 通道

```java
void disconnect(OperationDelegateCallBack callBack);
```

**参数说明**

|   参数    | 说明 |
| -------- | --- |
| callBack |  操作结果回调   |

**示例代码**

```java
mCameraP2P.connect(new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data) {
        //连接成功
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode) {
        //连接失败
    }
});
```


### 实时播放视频

P2P连接成功之后，就能进行实时视频播放了。

**接口说明**

开始播放实时视频

```java
void startPreview(int clarity, OperationDelegateCallBack callBack);
```

停止播放实时视频

```java
int stopPreview(OperationDelegateCallBack callBack);
```

**参数说明**

|   参数    |    说明     |
| -------- | ----------- |
| clarity  | 清晰度模式   |
| callBack | 操作结果回调 |


**清晰度模式**

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

**示例代码**

```java
mCameraP2P.startPreview(new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data) {
        //开始播放实时视频成功
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode) {
        //开始播放实时视频失败
    }
});
```

> 注意：startPreview 成功回调之后，onReceiveFrameYUVData 回调会开始接收视频数据，并抛给业务层。


### 销毁 ICameraP2P 对象

destroy 销毁对象，不再使用 camera 功能的时候，一定要调用 destroy

**示例代码**

```java
TuyaSmartCameraP2PFactory.onDestroyTuyaSmartCamera();   
```

## 流程图

<img src="./images/live_process_image.jpg" style="zoom:60%;" />

