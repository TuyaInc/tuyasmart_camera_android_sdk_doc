# TuyaCameraView 播放器控件组件



## 描述

TuyaCameraView 是一个播放器控件

## 方法

#### 构造播放器实例

```
public void createVideoView(int provider);
```

##### 参数说明

| 参数     | 说明                            |
| -------- | ------------------------------- |
| provider | 设备类型。从 DeviceBean 中获取到的 p2pType 值 |

#### 获取播放器实例

```
public Object createdView();
```

获得已构造的播放器实例。

#### 设置回调

```
public void setCameraViewCallback(CreateVideoViewCallback callback);
```

##### 参数说明

| 参数                    | 说明           |
| ----------------------- | -------------- |
| CreateVideoViewCallback | 播放器操作回调 |

> CreateVideoViewCallback 操作回调方法，参考如下：

| 回调            | 说明                     |
| --------------- | ------------------------ |
| onCreated       | 内部构造播放器完成时回调 |
| videoViewClick  | 点击播放器时回调         |
| startCameraMove | 拖动播放器时回调         |
| onActionUP      | 拖动播放器结束回调       |

#### 方向操作

```java
public void setOnRenderDirectionCallback(OnRenderDirectionCallback var1) {
  this.onRenderDirectionCallback = var1;
}
```

##### 参数说明

| 参数                      | 说明         |
| :------------------------- | :------------ |
| OnRenderDirectionCallback | 方向操作回调 |

> OnRenderDirectionCallback 操作回调方法，参考如下：

| 方法     | 说明     |
| :-------- | :-------- |
| onLeft   | 向左操作 |
| onRight  | 向右操作 |
| onUp     | 向上操作 |
| onDown   | 向下操作 |
| onCancel | 结束操作 |

**示例代码**

xml 布局文件：

```xml
<com.tuya.smart.camera.middleware.widget.TuyaCameraView
  android:id="@+id/camera_video_view"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
/>

```

```java
···
mVideoView = findViewById(R.id.camera_video_view);
mVideoView.setCameraViewCallback(this);
mVideoView.createVideoView(sdkProvider);
mVideoView.setOnRenderDirectionCallback(new OnRenderDirectionCallback() {

    @Override
    public void onLeft() {
        mDeviceControl.publishCameraDps(DpPTZControl.ID, PTZDirection.LEFT.getDpValue());
    }

    @Override
    public void onRight() {
        mDeviceControl.publishCameraDps(DpPTZControl.ID,PTZDirection.RIGHT.getDpValue());

    }

    @Override
    public void onUp() {
        mDeviceControl.publishCameraDps(DpPTZControl.ID,PTZDirection.UP.getDpValue());

    }

    @Override
    public void onDown() {
        mDeviceControl.publishCameraDps(DpPTZControl.ID,PTZDirection.DOWN.getDpValue());

    }

    @Override
    public void onCancel() {
        mDeviceControl.publishCameraDps(DpPTZStop.ID,true);

    }
});
···
// TuyaCameraView 构造 IMonitorView 完成时回调
@Override
public void onCreated(Object view) {
  if (null != mCameraP2P){
    // 为 ICameraP2P 绑定 IMonitorView
    mCameraP2P.generateCameraView(view);
  }
}
```



