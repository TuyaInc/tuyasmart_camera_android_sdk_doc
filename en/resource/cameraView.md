# camera player controls



TuyaMonitorView is a player controls, which is fit for 1 and 2 p2p type. show the camera image according to p2ptype and soft/hard decode parameter
 method：

> TuyaMonitorView.createVideoView(int p2pType, boolean isRunSoft， generate corresponding p2p player and use soft/hard decode


### get p2pType

```java
Map<String, Object> map = mDeviceBean.getSkills();
        if (map == null || map.size() == 0) {
            p2pType = -1;
        } else {
        	p2pType = (Integer) (map.get(P2PTYPE));
        }
```