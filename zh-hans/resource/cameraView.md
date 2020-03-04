# Monitor 播放器控件组件


## 介绍
Monitor是一个播放器控件

## 方法



#### 方向操作

```java
    public void setOnRenderDirectionCallback(OnRenderDirectionCallback var1) {
        this.onRenderDirectionCallback = var1;
    }
```

##### 参数说明

| 参数                      | 说明                               |
| ------------------------- | ---------------------------------- |
| OnRenderDirectionCallback | p2p类型，对应匹配相应设备的p2p能力 |

> OnRenderDirectionCallback操作回调方法，参考如下：

| 方法     | 说明     |
| -------- | -------- |
| onLeft   | 向左操作 |
| onRight  | 向右操作 |
| onUp     | 向上操作 |
| onDown   | 向下操作 |
| onCancel | 结束操作 |

### 示例代码：

xml布局文件：

```xml
        <com.tuya.smart.camera.ipccamerasdk.monitor.Monitor
            android:id="@+id/camera_video_view"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
             />
  
```

```java
··· 
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
···
```



