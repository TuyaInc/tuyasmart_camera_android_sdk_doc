# Monitor 



## Introduction

Monitor is a player control



## Method



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

| 方法     | Description      |
| -------- | ---------------- |
| onLeft   | 向左操作         |
| onRight  | Right operation  |
| onUp     | UP operation     |
| onDown   | Down operation   |
| onCancel | Cancel operation |



### Sampe Code 

view xml 

```xml
<com.tuya.smart.camera.ipccamerasdk.monitor.Monitor
                                                    android:id="@+id/camera_video_view"
                                                    android:layout_width="match_parent"
                                                    android:layout_height="match_parent"
                                                    />
```

```java
...
mVideoView = findViewById(R.id.camera_video_view);
mCameraP2P.generateCameraView(mVideoView);
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
```



