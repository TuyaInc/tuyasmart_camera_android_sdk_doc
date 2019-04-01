# camera 播放器控件



TuyaMonitorView是一个播放器控件，为适配p2ptype为1，2而产生的。根据传入的p2ptype和软硬解参数来显示对应的摄像头图像。

| p2pType | 描述                |
| ------- | ------------------- |
| 1       | 对应加载P2p 1.0设备 |
| 2       | 对应加载P2p 2.0设备 |

使用方法如下：

> TuyaMonitorView.createVideoView(int p2pType, boolean isRunSoft)  构建对应p2ptype的播放器，及使用软硬解。

### 获取p2pType

```java
Map<String, Object> map = mDeviceBean.getSkills();
        if (map == null || map.size() == 0) {
            p2pType = -1;
        } else {
        	p2pType = (Integer) (map.get(P2PTYPE));
        }
```