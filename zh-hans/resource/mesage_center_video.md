# 消息中心视频播放指南



## 属性和方法

### 属性

| 属性                | 说明                    |
| ------------------- | ----------------------- |
| 状态码              | 操作相关的状态码        |
| ITYCloudVideo       | 控制播放器操作API的接口 |
| TuyaCameraView      | 视频播放控件            |
| OnP2PCameraListener | 业务层所需的方法回调    |

### 方法


#### 注册监听
注册监听器，只有注册之后才能获取到视频播放的回调数据。

```java
void registorOnP2PCameraListener(OnP2PCameraListener listener);
```

##### 参数说明

| **参数**            | 说明             |
| ------------------- | ---------------- |
| OnP2PCameraListener | 视频播放回调数据 |

#### 注入播放器对象

注入播放器View，用来渲染视频画面

```java
void generateCloudCameraView(IRegistorIOTCListener view);
```

##### 参数说明

| **参数**              | 说明             |
| --------------------- | ---------------- |
| IRegistorIOTCListener | 视频播放回调数据 |

#### 创建云视频播放设备

```java
void createCloudDevice(String cachePath, String devId, OperationDelegateCallBack callBack);
```

##### 参数说明

| 参数                      | 说明         |
| ------------------------- | ------------ |
| cachePath                 | 缓存文件地址 |
| devId                     | 设备id       |
| OperationDelegateCallBack | 操作回调     |

##### 示例代码：

```kotlin
cloudVideo?.createCloudDevice(cachePath, devId, object : OperationDelegateCallBack {
            override fun onSuccess(sessionId: Int, requestId: Int, data: String) {
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_MEDIA_DEVICE, ICameraVideoPlayModel.OPERATE_SUCCESS, data))
            }

            override fun onFailure(sessionId: Int, requestId: Int, errCode: Int) {
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_MEDIA_DEVICE, ICameraVideoPlayModel.OPERATE_FAIL, errCode))
            }
        })
```

#### 开始播放视频

```java
void playVideo(String videoUrl, int startTime, String encryptKey, OperationCallBack callback, OperationCallBack playFinishedCallBack);
```

##### 参数说明

| 参数                 | 说明                    |
| -------------------- | ----------------------- |
| videoUrl             | 视频播放地址            |
| startTime            | 开始播放时间，初始值：0 |
| encryptKey           | 播放视频的秘钥          |
| callback             | 操作回调                |
| playFinishedCallBack | 结束播放的操作回调      |

##### 示例代码：

```kotlin
override fun playVideo(videoUrl: String, startTime: Int, encryptKey: String) {
        if (cloudVideo == null) {
            return
        }
        cloudVideo!!.playVideo(videoUrl, startTime, encryptKey, object : OperationCallBack {
            override fun onSuccess(sessionId: Int, requestId: Int, data: String, camera: Any) {
                playState = CloudPlayState.STATE_PLAYING
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_PLAY, ICameraVideoPlayModel.OPERATE_SUCCESS))
            }

            override fun onFailure(sessionId: Int, requestId: Int, errCode: Int, camera: Any) {
                playState = CloudPlayState.STATE_ERROR
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_PLAY, ICameraVideoPlayModel.OPERATE_FAIL))
            }
        }, object : OperationCallBack {
            override fun onSuccess(sessionId: Int, requestId: Int, data: String, camera: Any) {
                playState = CloudPlayState.STATE_COMPLETED
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_STOP, ICameraVideoPlayModel.OPERATE_SUCCESS))
            }

            override fun onFailure(sessionId: Int, requestId: Int, errCode: Int, camera: Any) {
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_STOP, ICameraVideoPlayModel.OPERATE_FAIL))
            }
        })
    }
```

#### 暂停播放

```java
void pauseVideo(OperationCallBack callback);
```

##### 参数说明

| **参数**                  | **说明** |
| ------------------------- | -------- |
| OperationDelegateCallBack | 操作回调 |

##### 示例代码：

```kotlin
override fun pauseVideo() {
        if (cloudVideo == null) {
            return
        }
        cloudVideo!!.pauseVideo(object : OperationCallBack {
            override fun onSuccess(sessionId: Int, requestId: Int, data: String, camera: Any) {
                playState = CloudPlayState.STATE_PAUSED
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_PAUSE, ICameraVideoPlayModel.OPERATE_SUCCESS))
            }

            override fun onFailure(sessionId: Int, requestId: Int, errCode: Int, camera: Any) {
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_PAUSE, ICameraVideoPlayModel.OPERATE_FAIL))
            }
        })
    }
```

#### 设置视频播放的声音开关

```java
void setCloudVideoMute(int mute, OperationDelegateCallBack callBack);
```

##### 参数说明

| **参数**                  | **说明**                                                     |
| ------------------------- | ------------------------------------------------------------ |
| mute                      | 拾音器模式: ICameraP2P.MUTE/ICameraP2P.UNMUTE（静音/非静音） |
| OperationDelegateCallBack | 操作回调                                                     |

##### 示例代码：

```kotlin
    private fun setCloudVideoMute(voiceMode: Int) {
        if (cloudVideo == null) {
            return
        }
        cloudVideo!!.setCloudVideoMute(voiceMode, object : OperationDelegateCallBack {

            override fun onSuccess(sessionId: Int, requestId: Int, data: String) {
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_MUTE, IPanelModel.ARG1_OPERATE_SUCCESS, data))
            }

            override fun onFailure(sessionId: Int, requestId: Int, errCode: Int) {
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_MUTE, IPanelModel.ARG1_OPERATE_FAIL))
            }
        })
    }
```



#### 继续播放

```java
void resumeVideo(OperationCallBack callback);
```

##### 参数说明

| **参数**                  | **说明** |
| ------------------------- | -------- |
| OperationDelegateCallBack | 操作回调 |

##### 示例代码：

```kotlin
    override fun resumeVideo() {
        if (cloudVideo == null) {
            return
        }
        cloudVideo!!.resumeVideo(object : OperationCallBack {
            override fun onSuccess(sessionId: Int, requestId: Int, data: String, camera: Any) {
                playState = CloudPlayState.STATE_PLAYING
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_RESUME, ICameraVideoPlayModel.OPERATE_SUCCESS))
            }

            override fun onFailure(sessionId: Int, requestId: Int, errCode: Int, camera: Any) {
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_RESUME, ICameraVideoPlayModel.OPERATE_FAIL))
            }
        })
    }
```

#### 停止播放

```java
void stopVideo(OperationCallBack callback);
```

##### 参数说明

| **参数**                  | **说明** |
| ------------------------- | -------- |
| OperationDelegateCallBack | 操作回调 |

##### 示例代码：

```kotlin
    override fun stopVideo() {
        if (cloudVideo == null) {
            return
        }
        cloudVideo!!.stopVideo(object : OperationCallBack {
            override fun onSuccess(sessionId: Int, requestId: Int, data: String, camera: Any) {
                playState = CloudPlayState.STATE_STOP
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_STOP, ICameraVideoPlayModel.OPERATE_SUCCESS))
            }

            override fun onFailure(sessionId: Int, requestId: Int, errCode: Int, camera: Any) {
                mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_STOP, ICameraVideoPlayModel.OPERATE_FAIL))
            }
        })
    }
```

#### 销毁云视频播放设备

```java
void deinitCloudVideo();
```

#### 视频数据回调

```kotlin
override fun onReceiveFrameYUVData(sessionId: Int, y: ByteBuffer, u: ByteBuffer, v: ByteBuffer, width: Int, height: Int, nFrameRate: Int, nIsKeyFrame: Int, timestamp: Long, nProgress: Long, nDuration: Long, camera: Any)
```

##### 参数说明

| 参数        | 说明                             |
| ----------- | -------------------------------- |
| sessionId   | \                                |
| Y           | 视频Y数据                        |
| u           | 视频U数据                        |
| v           | 视频V数据                        |
| width       | 视频画面的宽                     |
| height      | 视频画面的高                     |
| nFrameRate  | 帧率                             |
| nIsKeyFrame | 是否I帧                          |
| timestamp   | 时间戳                           |
| nProgress   | 时间进度(消息中心视频播放的进度) |
| nDuration   | 时长(消息中心视频播放时长)       |
| camera      | \                                |

  > 在这里的视频播放只要关心nProgress，nDuration即可

##### 示例代码：

  ```kotlin
      override fun onReceiveFrameYUVData(sessionId: Int, y: ByteBuffer, u: ByteBuffer, v: ByteBuffer, width: Int, height: Int, nFrameRate: Int, nIsKeyFrame: Int, timestamp: Long, nProgress: Long, nDuration: Long, camera: Any) {
          val map = HashMap<String, Long>(2)
          map["progress"] = nProgress
          map["duration"] = nDuration
          mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_INFO, ICameraVideoPlayModel.OPERATE_SUCCESS, map))
      }
  
  ```




## 流程图

![](./images/video_play_flow.png)



