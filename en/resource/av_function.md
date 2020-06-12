## AV Functions

The SDK provides some additional audio and video capabilities when play live video or playback.

### Recording

Once the video has been successfully played (either live or playback), you can record the video currently playing to the phone.

#### Start video recording

**Declaration**

```java
int startRecordLocalMp4(String folderPath, String fileName, Context context, OperationDelegateCallBack callBack);
```

> Note: Recording video requires write card permission

**Parameter**


| Parameter  |       Description        |
| ---------- | ------------------------ |
| folderPath | file path to save video |
| fileName   | file name                |
| context    | Context                  |
| callBack   | result callback         |

**Example**

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

#### stop video recording

**Declaration**

```java
int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter |   Description   |
| --------- | ---------------- |
| callback  | result callback |

**Example**

```java
mCameraP2P.stopRecordLocalMp4(new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data) {

    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode) {

    }
});
```

### Video snapshot

When the video starts to play successfully (it can be live video or video playback), you can take a screenshot of the currently displayed video image.

**Declaration**

```java
int snapshot(String absoluteFilePath, Context context, PLAYMODE playmode, OperationDelegateCallBack callBack);
```


**Play mode**

```java
enum PLAYMODE {
    LIVE(0), PLAYBACK(1);
}
```

**Parameter**


|    Parameter     |     Description      |
| ----------------- | --------------------- |
| absoluteFilePath | Picture storage path |
| context           | Context               |
| playmode           | Play mode               |
| callback           | Result callback             |

**Example**


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
        mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_SUCCESS, data));
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode) {
        mHandler.sendMessage(MessageUtil.getMessage(MSG_SCREENSHOT, ARG1_OPERATE_FAIL));
    }
});
```

### Video sound

When the video is successfully played (either live or playback), the video sound can be turned on, with the default sound turned off.

**Example**

```java
int mute;
mute = previewMute == ICameraP2P.MUTE ? ICameraP2P.UNMUTE : ICameraP2P.MUTE;
mCameraP2P.setMute(ICameraP2P.PLAYMODE.LIVE, mute, new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    //data is the result value after the operation
    previewMute = Integer.valueOf(data);
    mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_SUCCESS));
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
    mHandler.sendMessage(MessageUtil.getMessage(MSG_MUTE, ARG1_OPERATE_FAIL));
  }
});
```

### Talk to camera

After the p2p connection is successful, the real-time calling function with the device can be enabled. Before starting talk to device, we need to ensure that the App has gained access to the phone's microphone.

#### Start talck

Open the audio channel from App to camera.

**Example**

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

#### Stop talck

Close the audio channel from App to camera.

**Example**

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

#### Two-way talk

In the video live broadcast, turn on the video sound. At this time, the sound played is the human voice and environmental sound collected by the camera in real time. Then, open the sound channel from App to the camera to implement two-way talk.

> Some cameras may not have speakers or pickups, and such cameras are not capable of two-way talk.

#### One-way talk

The one-way talk function needs the developer to implement control. When the talk is on, turn off the video sound. After the talk is off, turn on the video sound again.

### Definition switching

In the video live, you can switch the definition (a few cameras only support one kind of definition), currently only high-definition and standard clarity two kinds of definition, and only when the video live support. A memory card video recording saves only one definition stream of video at the time of recording.

#### Get definition

Get the sharpness of the image transmitted from the camera.

**Example**

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

> note: After the preview screen comes out, call this function

#### Set Video Clarity

Set the sharpness of the image played by the camera.

**Sample code**

```java
mCameraP2P.setVideoClarity(videoClarity == ICameraP2P.HD ? ICameraP2P.STANDEND : ICameraP2P.HD, new OperationDelegateCallBack() {
    @Override
    public void onSuccess(int sessionId, int requestId, String data) {
        videoClarity = Integer.valueOf(data);
        mHandler.sendMessage(MessageUtil.getMessage(MSG_GET_CLARITY, ARG1_OPERATE_SUCCESS));
    }

    @Override
    public void onFailure(int sessionId, int requestId, int errCode) {
        mHandler.sendMessage(MessageUtil.getMessage(MSG_GET_CLARITY, ARG1_OPERATE_FAIL));
    }
});
```


