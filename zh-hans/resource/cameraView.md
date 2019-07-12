# camera 播放器控件



TuyaMonitorView是一个播放器控件，为适配provider为1，2而产生的。根据传入的p2ptype和软硬解参数来显示对应的摄像头图像。

| provider | 描述                |
| ------- | ------------------- |
| 2       | 对应加载P2p 2.0设备 |

使用方法如下：

> TuyaMonitorView.createVideoView(int provider)  构建对应provider的播放器。

### 获取provider

```java
Map<String, Object> map = mDeviceBean.getSkills();
        if (map == null || map.size() == 0) {
            provider = -1;
        } else {
        	provider = (Integer) (map.get("p2pType"));
        }
```