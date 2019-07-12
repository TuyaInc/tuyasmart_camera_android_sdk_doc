# camera player controls



TuyaMonitorView is a player controls, which is fit for 1 and 2 p2p type. show the camera image according to p2ptype and soft/hard decode parameter

| p2pType | description                |
| ------- | ------------------- |
| 2       | match device for P2p 2.0 |

 method：

> TuyaMonitorView.createVideoView(int provider), Build the VideoView corresponding to the provider.




### get p2pType

```java
Map<String, Object> map = mDeviceBean.getSkills();
        if (map == null || map.size() == 0) {
            p2pType = -1;
        } else {
        	p2pType = (Integer) (map.get(P2PTYPE));
        }
```