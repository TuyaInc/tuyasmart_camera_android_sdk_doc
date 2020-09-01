# TuyaCameraView 



## Introduction

TuyaCameraView is a player control



## Method


#### Construct player instance

```
public void createVideoView(int provider);
```

##### Parameter Description

| Parameter     | Description                            |
| -------- | ------------------------------- |
| provider | device type。the p2pType value, obtained from DeviceBean |

#### Get player instance

```
public Object createdView();
```

#### Set callback

```
public void setCameraViewCallback(CreateVideoViewCallback callback);
```

##### Parameter Description

| Parameter                    | Description           |
| ----------------------- | -------------- |
| CreateVideoViewCallback | player operation callback |

> CreateVideoViewCallback The reference is as follows：

|     function        | Description                     |
| --------------- | ------------------------ |
| onCreated       | Callback when the internally constructed player is completed |
| videoViewClick  | Callback when the player is clicked         |
| startCameraMove | Callback when dragging the player       |
| onActionUP      | Callback when dragging ends       |

### Direction operation

```java
public void setOnRenderDirectionCallback(OnRenderDirectionCallback callback) {
  this.onRenderDirectionCallback = callback;
}
```

##### **Parameter Description**

| Parameter                 | Description                  |
| ------------------------- | ---------------------------- |
| OnRenderDirectionCallback | direction operation callback |

> OnRenderDirectionCallback The reference is as follows：

| function     | Description      |
| -------- | ---------------- |
| onLeft   | Left operation         |
| onRight  | Right operation  |
| onUp     | UP operation     |
| onDown   | Down operation   |
| onCancel | Cancel operation |



### Sampe Code 

view xml 

```xml
<com.tuya.smart.camera.middleware.widget.TuyaCameraView
    android:id="@+id/camera_video_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    />
```

```java
...
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
// CreateVideoViewCallback callback when IMonitorView construction is complete
@Override
public void onCreated(Object view) {
  if (null != mCameraP2P){
    // Bind IMonitorView to ICameraP2P
    mCameraP2P.generateCameraView(view);
  }
}
```



