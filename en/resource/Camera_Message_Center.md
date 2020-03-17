# Camera Message Center



## Introduction

The camera message center records the picture data, video data of the camera detection alarm and data of someone visiting.



## Module Import

```groovy
implementation 'com.tuya.smart:tuyasmart-ipc-camera-message:3.13.0r128'
```



## Message Center List Get Data API



### queryAlarmDetectionDaysByMonth

Get the list of specific dates with messages in the specified month of the camera message center.

```java
void queryAlarmDetectionDaysByMonth(String jsonParams, ResultListener<JSONArray> listener)
```

**Parameter Description**

| Parameter | Parameter Type | Parameter requirements | Description                                             |
| --------- | -------------- | ---------------------- | ------------------------------------------------------- |
| msgSrcId  | String         | Need                   | Device Id                                               |
| month     | String         | Need                   | year and month, with-in the middle, for example: 2019-8 |
| timeZone  | String         | Need                   | Time zone, for example: Asia / Shanghai                 |

**Sample Code**

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



### queryAlarmDetectionClassify

Get message type

```java
void queryAlarmDetectionClassify(String devId, ResultListener<ArrayList<CameraMessageClassifyBean>> listener)
```

**Parameter Description**

| Parameter | Parameter Type | Parameter requirements | Description |
| --------- | -------------- | ---------------------- | ----------- |
| devId     | String         | Need                   | Device Id   |

**Sample Code**

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





### getAlarmDetectionMessageList

Get message center specific data

```java
void getAlarmDetectionMessageList(String json, ResultListener<JSONObject> listener)
```

**Parameter Description**

| Parameter | Parameter Type | Parameter requirements | Description                                                  |
| :-------: | -------------- | ---------------------- | ------------------------------------------------------------ |
| msgSrcId  | String         | Need                   | device Id                                                    |
|  msgType  | Int            | Need                   | Message Type, mesType = 4                                    |
|   limit   | Int            | Need                   | Limit                                                        |
|  offset   | Int            | Need                   | Offset                                                       |
| startTime | Long           | Need                   | Start time,  Unix timestamp, seconds                         |
|  endTime  | Long           | Need                   | End time , Unix timestamp, seconds                           |
| msgCodes  | String[]       | /                      | If not, all types of data are requested by default, if they are, then the corresponding type of message data is requested (for the message array, please refer to the message type interface introduction) |

**Sample Code**

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

CameraMessageBean：

**Parameter Description**

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



### deleteAlarmDetectionMessageList

delete Message 

```java
void deleteAlarmDetectionMessageList(String ids, ResultListener<Boolean> listener)
```

#### **Parameter Description**

| Parameter | Parameter Type | Parameter requirements | Description                                      |
| :-------: | -------------- | ---------------------- | ------------------------------------------------ |
|    ids    | String         | Need                   | String, multiple message ids separated by commas |

**Sample Code**

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



