

# Detection alarm


Tuya smart camera usually has the ability of detecting alarm. There are two main types of alarm detection, sound detection and motion detection. When the device detects a sound or object moving, it sends a warning message, and if your App is integrated with push, it will also receive a push notification.  Integrate push refer to[ Integrate push](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/Push.html).

The alarm message usually comes with a screenshot of the current video.

Direct-powered doorbell equipment, has ability to provide video messages. When someone rings the doorbell, the device can upload a video message, which is also picked up by an alarm message with a six-second encrypted video.

## Message List


### Module Import

```groovy
implementation 'com.tuya.smart:tuyasmart-ipc-camera-message:3.13.0r128'
```

### Message calendar

You can use the SDK to look up the date of the alarm message in a certain year and a certain month, so that it can be displayed visually on the calendar.

**Declaration**


The camera message center records the picture data, video data of the camera detection alarm and data of someone visiting.



```java
void queryAlarmDetectionDaysByMonth(String jsonParams, ResultListener<JSONArray> listener)
```

**Parameter**


| Parameter | Parameter Type | Parameter requirements | Description                                             |
| --------- | -------------- | ---------------------- | ------------------------------------------------------- |
| msgSrcId  | String         | Need                   | Device Id                                               |
| month     | String         | Need                   | year and month, with-in the middle, for example: 2019-8 |
| timeZone  | String         | Need                   | Time zone, for example: Asia / Shanghai                 |

**Example**


```java
JSONObject object = new JSONObject();
        object.put("msgSrcId", devId);
        object.put("timeZone", TimeZoneUtils.getTimezoneGCMById(TimeZone.getDefault().getID()));
        object.put("month", year + "-" + month);
        messageBusiness.queryAlarmDetectionDaysByMonth(object.toJSONString(),
                new Business.ResultListener<JSONArray>() {
                    @Override
                    public void onFailure(BusinessResponse businessResponse, JSONArray objects, String s) {
                        mHandler.sendEmptyMessage(ALARM_DETECTION_DATE_MONTH_FAILED);
                    }

                    @Override
                    public void onSuccess(BusinessResponse businessResponse, JSONArray objects, String s) {
                        List<Integer> dates = JSONArray.parseArray(objects.toJSONString(), Integer.class);
                        if (dates.size() > 0){
                            day = dates.get(0);
                        }
                        mHandler.sendEmptyMessage(ALARM_DETECTION_DATE_MONTH_SUCCESS);
                    }
                });
```

Example of returned data structure: (specifically based on actual request):

```json
{
	"result": ["20"],
	"t": 1564390878414,
	"success": true,
	"status": "ok"
}
```


### Message type

There are many types of detection alarm messages according to the trigger mode, and some types can be divided into a large category. The Camera SDK provides a list of default classifications to facilitate querying alarm messages by classification.

**Declaration**

Get message type

```java
void queryAlarmDetectionClassify(String devId, ResultListener<ArrayList<CameraMessageClassifyBean>> listener)
```

**Parameter**


| Parameter | Parameter Type | Parameter requirements | Description |
| --------- | -------------- | ---------------------- | ----------- |
| devId     | String         | Need                   | Device Id   |

**Example**


```java
public void queryCameraMessageClassify(String devId) {
    if (messageBusiness != null) {
        messageBusiness.queryAlarmDetectionClassify(devId, new Business.ResultListener<ArrayList<CameraMessageClassifyBean>>() {
            @Override
            public void onFailure(BusinessResponse businessResponse, ArrayList<CameraMessageClassifyBean> cameraMessageClassifyBeans, String s) {
                mHandler.sendEmptyMessage(MOTION_CLASSIFY_FAILED);
            }

            @Override
            public void onSuccess(BusinessResponse businessResponse, ArrayList<CameraMessageClassifyBean> cameraMessageClassifyBeans, String s) {
                selectClassify = cameraMessageClassifyBeans.get(0);
                mHandler.sendEmptyMessage(MOTION_CLASSIFY_SUCCESS);
            }
        });
    }
}
```

Example of returned data structure: (specifically based on actual request):

```json
\{
	"result": [{
		"describe": "All"
	}, {
		"describe": "Motion",
		"msgCode": ["ipc_motion", "ipc_linger", "ipc_passby", "ipc_human", "ipc_car", "ipc_cat", "MOVEMENT_DETECT_MSG"]
	}, {
		"describe": "Talk",
		"msgCode": ["ipc_connected", "ipc_unconnected", "ipc_refuse", "ipc_doorbell", "ipc_leave_msg"]
	}],
	"t": 1564390878289,
	"success": true,
	"status": "ok"
}
```

CameraMessageClassifyBean

| Parameter | Description         |
| --------- | ------------------- |
| describe  | message description |
| msgCode   | /                   |
| selected  | /                   |


### Message list

Get message center specific data

**Declaration**


```java
void getAlarmDetectionMessageList(String json, ResultListener<JSONObject> listener)
```
**Parameter**

| Parameter | Parameter Type | Parameter requirements | Description                                                  |
| :-------: | -------------- | ---------------------- | ------------------------------------------------------------ |
| msgSrcId  | String         | Need                   | device Id                                                    |
|  msgType  | Int            | Need                   | Message Type, mesType = 4                                    |
|   limit   | Int            | Need                   | Limit                                                        |
|  offset   | Int            | Need                   | Offset                                                       |
| startTime | Long           | Need                   | Start time,  Unix timestamp, seconds                         |
|  endTime  | Long           | Need                   | End time , Unix timestamp, seconds                           |
| msgCodes  | String[]       | /                      | If not, all types of data are requested by default, if they are, then the corresponding type of message data is requested (for the message array, please refer to the message type interface introduction) |

**Example**

```java
if (null != messageBusiness){
    long startTime = DateUtils.getTodayStart(day);
    long endTime = DateUtils.getTodayEnd(day) - 1L;
    JSONObject object = new JSONObject();
    object.put("msgSrcId", devId);
    object.put("startTime", startTime);
    object.put("endTime", endTime);
    object.put("msgType", 4);
    object.put("limit", 30);
    object.put("keepOrig", true);
    object.put("offset", offset);
    if (null != selectClassify) {
        object.put("msgCodes", selectClassify.getMsgCode());
    }
    messageBusiness.getAlarmDetectionMessageList(object.toJSONString(), new Business.ResultListener<JSONObject>() {
        @Override
        public void onFailure(BusinessResponse businessResponse, JSONObject jsonObject, String s) {
            mHandler.sendMessage(MessageUtil.getMessage(MSG_GET_ALARM_DETECTION, ARG1_OPERATE_FAIL));
        }

        @Override
        public void onSuccess(BusinessResponse businessResponse, JSONObject jsonObject, String s) {
            List<CameraMessageBean> msgList;
            try {
                msgList = JSONArray.parseArray(jsonObject.getString("datas"), CameraMessageBean.class);
            } catch (Exception e) {
                msgList = null;
            }
            if (msgList != null) {
                offset += msgList.size();
                mHandler.sendMessage(MessageUtil.getMessage(MSG_GET_ALARM_DETECTION, ARG1_OPERATE_SUCCESS));
            } else {
                mHandler.sendMessage(MessageUtil.getMessage(MSG_GET_ALARM_DETECTION, ARG1_OPERATE_FAIL));
            }
        }
    });
}
```

Example of returned data structure: (specifically based on actual request):

```json
 {
	"result" : {
		"datas" : [ {
		"actionURL" : "tuyaSmart://panel?devId=6c659ecc586a7da3b5pawj",
		"msgContent" : "switch open",
		"msgType" : 4,
		"time" : 1512639767,
		"icon" : "https://images.tuyacn.com/smart/product_icon/kt.png",
		"dateTime" : "2017-12-7 17:42",
		"id" : 3261004,
		"msgSrcId" : "6c659ecc586a7da3b5pawj",
    "msgTypeContent" : "test"
}, {
     "actionURL" : "tuyaSmart://panel?devId=6c659ecc586a7da3b5pawj",
		"msgContent" : "switch open",
		"msgType" : 4,
		"time" : 1511420861,
    "icon" : "https://images.tuyacn.com/smart/product_icon/kt.png",
		"dateTime" : "2017-11-23 15:07",
    "id" : 2967004,
    "msgSrcId" : "6c659ecc586a7da3b5pawj",
		"msgTypeContent" : "test"
} ],
	"totalCount" : 2
},
	"t" : 1539243923807,
	"success" : true,
	"status" : "ok"
}
```

#### CameraMessageBean data model

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| dateTime       | message create date                                          |
| msgTypeContent | /                                                            |
| msgType        | /                                                            |
| time           | message create time stamp                                    |
| attachPics     | Picture of message event, array format, take the first data  |
| attachVideos   | Video data of message event, array format, take the first data |
| actionURL      | /                                                            |
| id             | /                                                            |
| msgSrcId       | message ID                                                   |


### Batch delete alarm message.

**Declaration**

delete Message 

```java
void deleteAlarmDetectionMessageList(String ids, ResultListener<Boolean> listener)
```

**Parameter**


| Parameter | Parameter Type | Parameter requirements | Description                                      |
| :-------: | -------------- | ---------------------- | ------------------------------------------------ |
|    ids    | String         | Need                   | String, multiple message ids separated by commas |

**Example**


```java
StringBuilder ids = new StringBuilder();
for (int i = 0; i < mWaitingDeleteCameraMessageList.size(); i++) {
    ids.append(mWaitingDeleteCameraMessageList.get(i).getId());
    if (i != mWaitingDeleteCameraMessageList.size() - 1) {
        ids.append(",");
    }
}
messageBusiness.deleteAlarmDetectionMessageList(ids.toString(), new Business.ResultListener<Boolean>() {
                @Override
                public void onFailure(BusinessResponse businessResponse, Boolean aBoolean, String s) {
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_DELETE_ALARM_DETECTION, ARG1_OPERATE_FAIL));
                }

                @Override
                public void onSuccess(BusinessResponse businessResponse, Boolean aBoolean, String s) {
                    mCameraMessageList.removeAll(mWaitingDeleteCameraMessageList);
                    mWaitingDeleteCameraMessageList.clear();
                    mHandler.sendMessage(MessageUtil.getMessage(MSG_DELETE_ALARM_DETECTION, ARG1_OPERATE_SUCCESS));
                }
            });
```

Example of returned data structure: (specifically based on actual request):

```json
{
	"result" : true,
	"t" : 1548058543873,
	"success" : true,
	"status" : "ok"
}
```

## Video message

### Register to listener

Register the listener. Only after registration can you get the callback data for video playback.

**Declaration**


```java
void registorOnP2PCameraListener(OnP2PCameraListener listener);
```

**Parameter**


| Parameter           | Description              |
| ------------------- | ------------------------ |
| OnP2PCameraListener | Video play callback data |

### Bind player

Inject player view to render video

**Declaration**

```java
void generateCloudCameraView(IRegistorIOTCListener view);
```

**Parameter**


| Parameter             | Description              |
| --------------------- | ------------------------ |
| IRegistorIOTCListener | Video play callback data |


### createCloudDevice

create Message VideoPlayer device 

**Declaration**


```java
void createCloudDevice(String cachePath, String devId, OperationDelegateCallBack callBack);
```
**Parameter**


| Parameter                 | Description           |
| ------------------------- | --------------------- |
| cachePath                 | cache Video file path |
| devId                     | device Id             |
| OperationDelegateCallBack | operation callback    |

**Example**


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

### Play video in alarm message

**Declaration**


```java
void playVideo(String videoUrl, int startTime, String encryptKey, OperationCallBack callback, OperationCallBack playFinishedCallBack);
```

**Parameter**


| Parameter            | Description                           |
| -------------------- | ------------------------------------- |
| videoUrl             | Video url                             |
| startTime            | Start playback time, initial value: 0 |
| encryptKey           | Key for playing video                 |
| callback             | operation callback                    |
| playFinishedCallBack | finish play callback                  |

**Example**


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

### Pause playing

**Declaration**


```java
void pauseVideo(OperationCallBack callback);
```

**Parameter**


| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | operation callback |

**Example**


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

### Resume playing

**Declaration**


```java
void resumeVideo(OperationCallBack callback);
```

**Parameter**


| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | operation callback |

**Example**


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

### Stop playing

**Declaration**


```java
void stopVideo(OperationCallBack callback);
```

**Parameter**


| Parameter                 | Description        |
| ------------------------- | ------------------ |
| OperationDelegateCallBack | operation callback |

**Example**


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

### Set Cloud Video Mute

**Declaration**


```java
void setCloudVideoMute(int mute, OperationDelegateCallBack callBack);
```

**Parameter**


| Parameter                 | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| mute                      | Pickup mode: ICameraP2P.MUTE / ICameraP2P.UNMUTE (mute / non-mute) |
| OperationDelegateCallBack | operation callback                                           |

**Example**


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


### Deinit Cloud Video

When cloud video is no longer used, do destruction

```java
void deinitCloudVideo();
```

### Callback video YUV data

```java
void onReceiveFrameYUVData(int sessionId, ByteBuffer y, ByteBuffer u, ByteBuffer v, int width, int height, int nFrameRate, int nIsKeyFrame, long timestamp, long nProgress, long nDuration, Object camera);
```

**Parameter**


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



