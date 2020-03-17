# Message center video play



## Properties and Method

### Properties

| Properties          | Description                                |
| ------------------- | ------------------------------------------ |
| Status Code         | Operation Status Code                      |
| ITYCloudVideo       | Interface to control player operation API  |
| Monitor             | video Component                            |
| OnP2PCameraListener | Method callback required by business layer |



### Method

#### Register to listener

Register the listener. Only after registration can you get the callback data for video playback.

```java
void registorOnP2PCameraListener(OnP2PCameraListener listener);
```

##### **Parameter Description**

| Parameter           | Description              |
| ------------------- | ------------------------ |
| OnP2PCameraListener | Video play callback data |

#### 

#### Bind player

Inject player view to render video

```java
void generateCloudCameraView(IRegistorIOTCListener view);
```

##### **Parameter Description**

| Parameter             | Description              |
| --------------------- | ------------------------ |
| IRegistorIOTCListener | Video play callback data |

#### 

### createCloudDevice

create Message VideoPlayer device 

```java
void createCloudDevice(String cachePath, String devId, OperationDelegateCallBack callBack);
```

##### **Parameter Description**

| Parameter                 | Description           |
| ------------------------- | --------------------- |
| cachePath                 | cache Video file path |
| devId                     | device Id             |
| OperationDelegateCallBack | operation callback    |

**Sample Code**

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



### Start Play Video

```java
void playVideo(String videoUrl, int startTime, String encryptKey, OperationCallBack callback, OperationCallBack playFinishedCallBack);
```

##### **Parameter Description**

| Parameter            | Description                           |
| -------------------- | ------------------------------------- |
| videoUrl             | Video url                             |
| startTime            | Start playback time, initial value: 0 |
| encryptKey           | Key for playing video                 |
| callback             | operation callback                    |
| playFinishedCallBack | finish play callback                  |

**Sample Code**

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



### Pause Video

```java
void pauseVideo(OperationCallBack callback);
```

##### **Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | operation callback |

**Sample Code**

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



### set Cloud Video Mute

```java
void setCloudVideoMute(int mute, OperationDelegateCallBack callBack);
```

##### **Parameter Description**

| Parameter                 | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| mute                      | Pickup mode: ICameraP2P.MUTE / ICameraP2P.UNMUTE (mute / non-mute) |
| OperationDelegateCallBack | operation callback                                           |

**Sample Code**

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



### Resume Video

```java
void resumeVideo(OperationCallBack callback);
```

##### **Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | operation callback |

**Sample Code**

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



### Stop Video

```java
void stopVideo(OperationCallBack callback);
```

##### **Parameter Description**

| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | operation callback |

**Sample Code**

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



### deinit Cloud Video

When cloud video is no longer used, do destruction

```java
void deinitCloudVideo();
```



### Callback video YUV data

```java
void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, int nFrameRate, int nIsKeyFrame, long timestamp, long nProgress, long nDuration, Object camera);
```

**Parameter Description**

| Parameter   | Description                                             |
| ----------- | ------------------------------------------------------- |
| sessionId   | p2p session Id                                          |
| y           | Video Y data                                            |
| u           | Video U data                                            |
| v           | Video V data                                            |
| width       | Video width                                             |
| height      | Video height                                            |
| timestamp   | Time stamp                                              |
| nFrameRate  | Frame rate                                              |
| nIsKeyFrame | Whether I frame                                         |
| nProgress   | ime progress (the progress of the message center video) |
| nDuration   | Duration (message center video duration)                |
| camera      | /                                                       |

> As long as the video here is concerned about nProgress, nDuration

**Sample Code**

```kotlin
override fun onReceiveFrameYUVData(sessionId: Int, y: ByteBuffer, u: ByteBuffer, v: ByteBuffer, width: Int, height: Int, nFrameRate: Int, nIsKeyFrame: Int, timestamp: Long, nProgress: Long, nDuration: Long, camera: Any) {
  val map = HashMap<String, Long>(2)
  map["progress"] = nProgress
  map["duration"] = nDuration
  mHandler.sendMessage(MessageUtil.getMessage(ICameraVideoPlayModel.MSG_CLOUD_VIDEO_INFO, ICameraVideoPlayModel.OPERATE_SUCCESS, map))
}
```



## Flow Chart

![](./images/video_play_flow.png)

