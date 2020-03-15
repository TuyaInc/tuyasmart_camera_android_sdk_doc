# 涂鸦IPC Camera SDK 主要物件


## 简介

**Tuya IPC Android SDK**提供了智能摄像机实时视频播放、设备存储卡录像播放，对正在播放的视频截图、录制视频，与摄像机实时通话等基础能力，并提供了视频帧解码后的YUV数据，开发者可以对视频数据进行二次处理。

**Tuya IPC Android SDK**主要物件如下表：


| 对象                    |                                 说明 |
| :----------------------- | -----------------------------------: |
| ICameraP2P                 | IOTCamera提供相关远程IP Camera的功能 |
| Monitor          |                         视频播放控件 |
| OnP2PCameraListener |                 业务层所需的方法回调 |
| OperationDelegateCallBack |                 p2p信令操作方法回调 |
| DecryptImageView |                 图片解密组件 |
| TimeBarView |                 时间刻度尺组件 |

