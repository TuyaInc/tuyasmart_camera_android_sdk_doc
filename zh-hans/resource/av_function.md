
## 音视频功能

除了实时视频直播，存储卡录像播放以外，Camera SDK 还提供了一些额外的音视频能力。

### 本地录制

当视频成功开始播放以后（可以是视频直播，也可以是录像回放），可以将当前正在播放的视频录制到手机中。

#### 开启视频录制

**接口说明**

```java
int startRecordLocalMp4(String folderPath, String fileName, Context context, OperationDelegateCallBack callBack);
```

> 注：录制视频需要写存储卡权限

**参数说明**

|    参数     |       说明       |
| ---------- | ---------------- |
| folderPath | 保存视频的文件路径 |
| fileName   | 保存视频的名称     |
| context    | 上下文            |
| callBack   | 操作回调          |

**示例代码**

```java
if (Constants.hasStoragePermission()) {
    String picPath = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
    File file = new File(picPath);
    if (!file.exists()) {
        file.mkdirs();
    }
    String fileName = System.currentTimeMillis() + ".mp4";
    videoPath = picPath + fileName;
    mCameraP2P.startRecordLocalMp4(picPath, fileName, CameraPanelActivity.this, new OperationDelegateCallBack() {
        @Override
        public void onSuccess(int sessionId, int requestId, String data) {
            isRecording = true;
            mHandler.sendEmptyMessage(MSG_VIDEO_RECORD_BEGIN);

        }

        @Override
        public void onFailure(int sessionId, int requestId, int errCode) {
            mHandler.sendEmptyMessage(MSG_VIDEO_RECORD_FAIL);
        }
    });
    recordStatue(true);
} else {
    Constants.requestPermission(CameraPanelActivity.this, Manifest.permission.WRITE_EXTERNAL_STORAGE, Constants.EXTERNAL_STORAGE_REQ_CODE, "open_storage");
} 
```

#### 停止视频录制

**接口说明**

```java
int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```

**参数说明**


|   参数    |   说明   |
| -------- | ------- |
| callBack | 操作回调 |

**示例代码**

```java
mCameraP2P.stopRecordLocalMp4(new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data) {
        //成功
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode) {
        //失败
    }
});
```

### 视频截图

截取实时视频的影像图片存储到手机 SD 卡上

**接口说明**

```java
int snapshot(String absoluteFilePath, Context context, PLAYMODE playmode, OperationDelegateCallBack callBack);
```

**播放模式**

```java
enum PLAYMODE {
    LIVE(0), PLAYBACK(1);
}
```

**参数说明**

|       参数        |    说明     |
| ----------------- | ----------- |
| absoluteFilePath | 图片存储路径 |
| context           | 上下文      |
| playmode         | 播放模式     |
| callBack         | 操作回调     |

**示例代码**

```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    String path = Environment.getExternalStorageDirectory().getAbsolutePath() + "/Camera/";
    File file = new File(path);
    if (!file.exists()) {
        file.mkdirs();
    }
    picPath = path;
}
mCameraP2P.snapshot(picPath, CameraPanelActivity.this, ICameraP2P.PLAYMODE.LIVE, new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data) {
     //data 返回的是文件路径
        mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_SUCCESS, data));
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode) {
        mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_FAIL));
    }
});
```

### 视频声音

当视频成功开始播放以后（可以是视频直播，也可以是录像回放），可以开启视频声音，默认声音是关闭状态。

**示例代码**

```java
int mute;
mute = previewMute == ICameraP2P.MUTE ? ICameraP2P.UNMUTE : ICameraP2P.MUTE;
mCameraP2P.setMute(ICameraP2P.PLAYMODE.LIVE, mute, new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    //data返回的是对应操作之后的结果值
    previewMute = Integer.valueOf(data);
    mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_SUCCESS));
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
    mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_FAIL));
  }
});
```

### 实时对讲

在 p2p 连接成功后，可以开启与设备的实时通话功能，在开始对讲前，需要确保 App 已获得手机麦克风的访问权限。

#### 开启对讲

打开手机声音传输给摄像机操作。

**示例代码**

```java
if (Constants.hasRecordPermission()) {
    mCameraP2P.startAudioTalk(new OperationDelegateCallBack() {
        @Override
        public void onSuccess(int sessionId, int requestId, String data) {
            isSpeaking = true;
            mHandler.sendMessage(MessageUtil.getMessage(MSG_TALK_BACK_BEGIN, ARG1_OPERATE_SUCCESS));
            ToastUtil.shortToast(CameraPanelActivity.this, "start talk success");
        }

        @Override
        public void onFailure(int sessionId, int requestId, int errCode) {
            isSpeaking = false;
            mHandler.sendMessage(MessageUtil.getMessage(MSG_TALK_BACK_BEGIN, ARG1_OPERATE_FAIL));
            ToastUtil.shortToast(CameraPanelActivity.this, "operation fail");

        }
    });
} else {
    Constants.requestPermission(CameraPanelActivity.this, Manifest.permission.RECORD_AUDIO, Constants.EXTERNAL_AUDIO_REQ_CODE, "open_recording");
}
```

#### 停止对讲

关闭手机声音传输给摄像机操作。

**示例代码**

```java
mCameraP2P.stopAudioTalk(new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data) {
        isSpeaking = false;
        mHandler.sendMessage(MessageUtil.getMessage(MSG_TALK_BACK_OVER, ARG1_OPERATE_SUCCESS));
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode) {
        isSpeaking = false;
        mHandler.sendMessage(MessageUtil.getMessage(MSG_TALK_BACK_OVER, ARG1_OPERATE_FAIL));

    }
});
```

> 注：对讲和录制是互斥的，而且只有在预览时可以开启对讲。


#### 双向对讲

在实时视频直播时，打开视频声音，此时播放的声音即为摄像机实时采集的人声与环境声音，此时打开 App 到摄像机的声音通道，即可实现双向对讲功能。

> 部分摄像机可能没有扬声器或者拾音器，此类摄像机无法实现双向对讲。

#### 单向对讲

单向对讲功能需要开发者来实现控制。在开启对讲的时候，关闭视频声音，关闭对讲后，再打开视频声音即可。

### 清晰度切换

在实时视频直播时，可以切换清晰度（少数摄像机只支持一种清晰度），目前只有高清和标清两种清晰度，且只有实时视频直播时才支持。存储卡视频录像在录制时只保存了一种清晰度的视频流。

#### 获取清晰度



获取摄像机传过来的影像清晰度。



**示例代码**



```java

mCameraP2P.getVideoClarity(new OperationDelegateCallBack() {

​    @Override

​    public void onSuccess(int sessionId, int requestId, String data) {

​        

​    }



​    @Override

​    public void onFailure(int sessionId, int requestId, int errCode) {



​    }

});

```


> 注意：预览画面出来后，调取该函数


#### 设置清晰度



设置摄像机播放的影像清晰度。



**示例代码**



```java

 mCameraP2P.setVideoClarity(videoClarity == ICameraP2P.HD ? ICameraP2P.STANDEND : ICameraP2P.HD, new OperationDelegateCallBack() {

​    @Override

​    public void onSuccess(int sessionId, int requestId, String data) {

​        videoClarity = Integer.valueOf(data);

​        mHandler.sendMessage(MessageUtil.getMessage(MSG_GET_CLARITY, ARG1_OPERATE_SUCCESS));

​    }



​    @Override

​    public void onFailure(int sessionId, int requestId, int errCode) {

​        mHandler.sendMessage(MessageUtil.getMessage(MSG_GET_CLARITY, ARG1_OPERATE_FAIL));

​    }

});

```




