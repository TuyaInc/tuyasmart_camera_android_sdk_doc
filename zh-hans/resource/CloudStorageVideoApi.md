#  云存储视频播放



## 描述

**ITYCloudCamera** 提供了云存储播放相关的API接口



## 使用方法

### 初始化，创建设备对象

```java
void createCloudDevice(String cachePath, String devId)
```

**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| cachePath | 缓存路径 |
| devId | 设备id |

**示例代码**

```java
cloudCamera.createCloudDevice(cachePath, devId);
```



### 注册云存储监听

```java
void registorOnP2PCameraListener(OnP2PCameraListener listener);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| listener | p2p 回调 |

**示例代码**

```java
cloudCamera.registorOnP2PCameraListener(this);
```



### 反注册云存储监听

```java
void removeOnP2PCameraListener();
```
**示例代码**

```java
cloudCamera.removeOnP2PCameraListener();
```



### 绑定播放组件 view

```java
void generateCloudCameraView(IRegistorIOTCListener view);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| view | 播放器组件 |

**示例代码**

```java
cloudCamera.generateCloudCameraView(mVideoView);
```



### 配置云存储数据 tags (开始播放云存前需要先进行配置)

```java
int configCloudDataTagsV1(String jsonStr, OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| jsonStr | 配置云存储数据 tags |
| callBack | 回调 |

**示例代码**

```java
cloudCamera.configCloudDataTagsV1(config, new OperationDelegateCallBack() {

  @Override
  public void onSuccess(int i, int i1, String s) {
    //成功之后开始播放
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



### 云存储播放

```java
void playCloudDataWithStartTime(long mStartTime, long mEndTime, boolean isEvent, String jsonAuthParams, String encryptKey, OperationCallBack callback, OperationCallBack playFinishedCallBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| mStartTime | 开始时间 |
| mEndTime | 结束时间，一般是这一天的结束时间 |
| isEvent | 是否是侦测事件，默认false |
| jsonAuthParams | 校验数据|
| encryptKey | 秘钥 |
| callBack | 播放回调 |
| playFinishedCallBack | 播放结束回调 |

**示例代码**

```java
cloudCamera.playCloudDataWithStartTime(startTime, endTime, isEvent,
                                       mAuthorityJson, mEncryptKey,
                                       new OperationCallBack() {
                                         @Override
                                         public void onSuccess(int sessionId, int requestId, String data, Object camera) {
                                           // 播放中的回调, playing
                                         }

                                         @Override
                                         public void onFailure(int sessionId, int requestId, int errCode, Object camera) {

                                         }
                                       }, new OperationCallBack() {
                                         @Override
                                         public void onSuccess(int sessionId, int requestId, String data, Object camera) {
                                           //播放完成的回调, playCompleted
                                         }

                                         @Override
                                         public void onFailure(int sessionId, int requestId, int errCode, Object camera) {
                                         }
                                       });
```



### 暂停播放

```java
int pausePlayCloudVideo(OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| callBack | 操作回调 |

**示例代码**

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



### 继续播放

```java
int resumePlayCloudVideo(OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| callBack | 操作回调 |

**示例代码**

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



### 停止播放

```java
int stopPlayCloudVideo(OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| callBack | 操作回调 |

**示例代码**

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



### 获取声音状态（是否静音）

```java
int getCloudMute();
```
**示例代码**

```java
cloudCamera.getCloudMute()
```



### 设置声音状态

``` java
void setCloudMute(final int mute, OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| mute | 是否静音 |
| callBack | 操作回调 |

**示例代码**

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



### 截图

```java
int snapshot(String absoluteFilePath, OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| absoluteFilePath | 文件地址 |
| callBack | 操作回调 |

**示例代码**

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



### 开始录制

```java
int startRecordLocalMp4(String folderPath, String fileName, OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| folderPath | 文件夹路径 |
| fileName | 文件名称 |
| callBack | 操作回调 |

**示例代码**

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



### 结束录制

```java
int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```
**参数说明**

| 参数                 | 说明               |
| :-------------------- | :------------------ |
| callBack | 操作回调 |

**示例代码**

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



### 反初始化

```java
void deinitCloudCamera();
```

**示例代码**

```java
cloudCamera.deinitCloudCamera();
```