# Cloud storage video play



## Introduction

**ITYCloudCamera** provides API interfaces related to cloud storage playback.



## Use

### Initialize and create device

```java
void createCloudDevice(String cachePath, String devId)
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| cachePath | Cache path  |
| devId     | device Id   |

**Sample Code**

```java
cloudCamera.createCloudDevice(cachePath, devId);
```



### registorOnP2PCameraListener

Register for Cloud camera listening

```java
void registorOnP2PCameraListener(OnP2PCameraListener listener);
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| listener  | Callback    |

**Sample Code**

```java
cloudCamera.registorOnP2PCameraListener(this);
```



### removeOnP2PCameraListener

Unregister Cloud Camera listening

```java
void removeOnP2PCameraListener();
```

**Sample Code**

```java
cloudCamera.removeOnP2PCameraListener(this);
```



### Binder video View

```java
void generateCloudCameraView(IRegistorIOTCListener view);
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| view      | Video view  |

**Sample Code**

```java
cloudCamera.generateCloudCameraView(mVideoView);
```



### configCloudDataTagsV1

Configure cloud storage data tags (requires configuration before starting to play cloud storage).

```java
int configCloudDataTagsV1(String jsonStr, OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter | Description                       |
| --------- | :-------------------------------- |
| jsonStr   | Configure cloud storage data tags |
| callBack  | Callback                          |

**Sample Code**

```java
cloudCamera.configCloudDataTagsV1(config, new OperationDelegateCallBack() {

  @Override
  public void onSuccess(int i, int i1, String s) {
    if (timePieceBeans.size() > 0) {
      int startTime = timePieceBeans.get(0).getStartTime();
      playCloudDataWithStartTime(startTime, (int) (getTodayEnd(startTime * 1000L) / 1000) - 1, true);
    }
  }

  @Override
  public void onFailure(int i, int i1, int i2) {

  }
});
```



### playCloudDataWithStartTime

Cloud storage play

```java
void playCloudDataWithStartTime(long mStartTime, long mEndTime, boolean isEvent, String jsonAuthParams, String encryptKey, OperationCallBack callback, OperationCallBack playFinishedCallBack);
```

**Parameter Description**

| Parameter            | Description                               |
| -------------------- | ----------------------------------------- |
| mStartTime           | Starting time                             |
| mEndTime             | End time, usually the end time of the day |
| isEvent              | Whether to detect events, default false   |
| jsonAuthParams       | Check data                                |
| encryptKey           | Secret key                                |
| callBack             | play callback                             |
| playFinishedCallBack | Play finish callback                      |

**Sample Code**

```java
cloudCamera.playCloudDataWithStartTime(startTime, endTime, isEvent,
                                       mAuthorityJson, mEncryptKey,
                                       new OperationCallBack() {
                                         @Override
                                         public void onSuccess(int sessionId, int requestId, String data, Object camera) {

                                         }

                                         @Override
                                         public void onFailure(int sessionId, int requestId, int errCode, Object camera) {

                                         }
                                       }, new OperationCallBack() {
                                         @Override
                                         public void onSuccess(int sessionId, int requestId, String data, Object camera) {

                                         }

                                         @Override
                                         public void onFailure(int sessionId, int requestId, int errCode, Object camera) {
                                         }
                                       });
```



### pausePlayCloudVideo

```java
int pausePlayCloudVideo(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| callBack  | call back   |

**Sample Code**

```java
cloudCamera.pausePlayCloudVideo(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
```



### resumePlayCloudVideo

```java
int resumePlayCloudVideo(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| callBack  | call back   |

**Sample Code**

```java
cloudCamera.resumePlayCloudVideo(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
```



### stopPlayCloudVideo

```java
int stopPlayCloudVideo(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| callBack  | call back   |

**Sample Code**

```java
cloudCamera.stopPlayCloudVideo(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
```



### getCloudMute

```java
int getCloudMute();
```

**Sample Code**

```java
cloudCamera.getCloudMute()
```



### setCloudMute

Set sound status

``` java
void setCloudMute(final int mute, OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| mute      | is mute     |
| callBack  | call back   |

**Sample Code**

```java
cloudCamera.setCloudMute(mute, new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    soundState = Integer.valueOf(data);
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
  }
});
```



### snapshot

```java
int snapshot(String absoluteFilePath, OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter        | Description |
| ---------------- | ----------- |
| absoluteFilePath | file path   |
| callBack         | call back   |

**Sample Code**

```java
cloudCamera.snapshot(IPCCameraUtils.recordSnapshotPath(devId), new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    Toast.makeText(CameraCloudStorageActivity.this, "snapshot success", Toast.LENGTH_SHORT).show();
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
  }
});
```



### startRecordLocalMp4

```java
int startRecordLocalMp4(String folderPath, String fileName, OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter  | Description |
| ---------- | ----------- |
| folderPath | folder path |
| fileName   | file name   |
| callBack   | Call back   |

**Sample Code**

```java
cloudCamera.startRecordLocalMp4(IPCCameraUtils.recordPath(devId), String.valueOf(System.currentTimeMillis()), new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    Toast.makeText(CameraCloudStorageActivity.this, "record start success", Toast.LENGTH_SHORT).show();
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
  }
});
```



### stopRecordLocalMp4

```java
int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| callBack  | call back   |

**Sample Code**

```java
cloudCamera.stopRecordLocalMp4(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    Toast.makeText(CameraCloudStorageActivity.this, "record end success", Toast.LENGTH_SHORT).show();
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
  }
});
```



### deinitCloudCamera

```java
void deinitCloudCamera();
```

**Sample Code**

```java
cloudCamera.deinitCloudCamera();
```

