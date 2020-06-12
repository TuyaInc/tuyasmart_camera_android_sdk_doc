## 存储卡回放

涂鸦 IPC 摄像机支持 SD卡录制功能，智能摄像机插入存储卡后，可以查看存储卡的信息和状态，并设置录像开关和模式，详情可以参考 [存储卡管理功能](./sd_card.md)。

存储卡回放需要 **Camera SDK** 通过云端服务器信息，接着创建 `ICameraP2P` 对象，然后进行 P2P 连接后，就可以播放摄像机 SD 卡录制下来的视频了。

### 视频片段

#### 获取有回放视频记录的日期

在开始回放前，需要获取到回放视频记录的信息。首先获取有回放视频记录的日期

**接口说明**

```java
void queryRecordDaysByMonth(int year, int month, OperationDelegateCallBack callBack);
```

**参数说明**


|      参数      |   说明    |
| -------------- | --------- |
| year           | 查询的年份 |
| month          | 查询的月份 |
| monthcallBack | 结果回调   |

**示例代码**

```java
int year = Integer.parseInt(substring[0]);
int mouth = Integer.parseInt(substring[1]);
queryDay = Integer.parseInt(substring[2]);
mCameraP2P.queryRecordDaysByMonth(year, mouth, new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    //data是获取到的月份数据
    MonthDays monthDays = JSONObject.parseObject(data, MonthDays.class);
    mBackDataMonthCache.put(mCameraP2P.getMonthKey(), monthDays.getDataDays());
    mHandler.sendMessage(MessageUtil.getMessage(MSG_DATA_DATE, ARG1_OPERATE_SUCCESS, data));
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
    mHandler.sendMessage(MessageUtil.getMessage(MSG_DATA_DATE, ARG1_OPERATE_FAIL));
  }
}); 
```

#### 获取某日的视频回放信息

获取到有用回放记录的日期后，根据日期获取当日的视频回放记录

**示例代码**

```java
int year = Integer.parseInt(substring[0]);
int mouth = Integer.parseInt(substring[1]);
int day = Integer.parseInt(substring[2]);
mCameraP2P.queryRecordTimeSliceByDay(year, mouth, day, new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {					//data是获取到的日期时间片段数据
    parsePlaybackData(data);
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {
    mHandler.sendEmptyMessage(MSG_DATA_DATE_BY_DAY_FAIL);
  }
});
```



### 视频播放

#### 开启回放

**示例代码**

```java
mCameraP2P.startPlayBack(timePieceBean.getStartTime(),
                         timePieceBean.getEndTime(),
                         timePieceBean.getStartTime(), new OperationDelegateCallBack() {
                           @Override
                           public void onSuccess(int sessionId, int requestId, String data){
                             isPlayback = true;
                           }

                           @Override
                           public void onFailure(int sessionId, int requestId, int errCode){
                             isPlayback = false;
                           }
                         }, new OperationDelegateCallBack() {
                           @Override
                           public void onSuccess(int sessionId, int requestId, String data){
                             isPlayback = false;
                           }

                           @Override
                           public void onFailure(int sessionId, int requestId, int errCode){
                             isPlayback = false;
                           }
                         });
```

#### 暂停回放

**示例代码**

```java
mCameraP2P.pausePlayBack(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    isPlayback = false;
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
```

#### 恢复回放

**示例代码**

   ```java
mCameraP2P.resumePlayBack(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {
    isPlayback = true;
  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
   ```

 

#### 结束回放

**示例代码**

   ```java
mCameraP2P.stopPlayBack(new OperationDelegateCallBack() {
  @Override
  public void onSuccess(int sessionId, int requestId, String data) {

  }

  @Override
  public void onFailure(int sessionId, int requestId, int errCode) {

  }
});
   ```

### 流程图

<img src="./images/playback_process.jpg" style="zoom:50%;" />


> 开启回放成功后，可以使用音视频功能，例如开启/停止视频录制、视频截图、开启/关闭视频声音，详情可参考 [音视频功能](./av_function.md)

