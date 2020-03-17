# 摄像机消息中心



## 简介

摄像机消息中心记录了摄像机侦测报警的图片数据、视频数据及有人来访的数据。



## 模块引用

```groovy
implementation 'com.tuya.smart:tuyasmart-ipc-camera-message:3.13.0r128'
```



## 消息中心列表获取数据API说明



### queryAlarmDetectionDaysByMonth 

获取摄像机消息中心指定月含有消息的具体日期列表

```java
void queryAlarmDetectionDaysByMonth(String jsonParams, ResultListener<JSONArray> listener)
```

**参数说明**

| 参数名称 | 类型   | 参数要求 | 描述                            |
| :------: | ------ | -------- | ------------------------------- |
| msgSrcId | String | 必须     | devId(设备 ID)                  |
|  month   | String | 必须     | 年月,中间以 - 拼接,例如: 2019-8 |
| timeZone | String | 必须     | 时区, 例如: Asia/Shanghai       |

然后将其**整体**包含在 **json** 字段中，例如：

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

返回数据结构示例:(具体以实际请求为准)

```json
*** JSON示例 ***
{
	"result": ["20"],
	"t": 1564390878414,
	"success": true,
	"status": "ok"
}
```



### queryAlarmDetectionClassify

获取消息类型

```java
void queryAlarmDetectionClassify(String devId, ResultListener<ArrayList<CameraMessageClassifyBean>> listener)
```

**参数说明**

| 参数名称 | 类型   | 出现要求 | 描述           |
| :------: | ------ | -------- | -------------- |
|  devId   | String | 必须     | devId(设备 ID) |

然后将其**整体**包含在 **json** 字段中,例如：

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

返回数据结构示例:(具体以实际请求为准)

```json
*** JSON示例 ***
{
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

返回数据封装在CameraMessageClassifyBean，格式说明如下：

| 参数     | 说明     |
| -------- | -------- |
| describe | 消息描述 |
| msgCode  | 无需关注 |
| selected | 无需关注 |





### getAlarmDetectionMessageList

获取消息中心具体数据

```java
void getAlarmDetectionMessageList(String json, ResultListener<JSONObject> listener)
```

#### 参数说明

| 参数名称  | 类型     | 出现要求 | 描述                                                         |
| :-------: | -------- | -------- | ------------------------------------------------------------ |
| msgSrcId  | String   | 必须     | devId(设备 ID)                                               |
|  msgType  | Int      | 必须     | 消息类型，目前只要传4即可                                    |
|   limit   | Int      | 必须     | 请求的数量                                                   |
|  offset   | Int      | 必须     | 请求的偏移量,用来做分页                                      |
| startTime | Long     | 必须     | 起始时间 传 Unix 时间戳,秒级                                     |
|  endTime  | Long     | 必须     | 结束时间 传 Unix 时间戳,秒级                                      |
| msgCodes  | String[] | 可选     | 如果不传,默认请求所有类型的数据,如果传了,则请求对应类型的消息数据(消息数组可以查看消息类型接口介绍) |

>  PS: 如果要查询某一天的全部消息数据，可以将结束时间和起始时间间隔一天的时间戳即可。然后将其**整体**包含在 **json** 字段中，例如：

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

返回数据结构示例:(具体以实际请求为准)

```json
*** JSON示例 ***
 {
	"result" : {
		"datas" : [ {
		"actionURL" : "tuyaSmart://panel?devId=6c659ecc586a7da3b5pawj",
		"msgContent" : "开关打开",
		"msgType" : 4,
		"time" : 1512639767,
		"icon" : "https://images.tuyacn.com/smart/product_icon/kt.png",
		"dateTime" : "2017-12-7 17:42",
		"id" : 3261004,
		"msgSrcId" : "6c659ecc586a7da3b5pawj",
    "msgTypeContent" : "告警测试"
}, {
     "actionURL" : "tuyaSmart://panel?devId=6c659ecc586a7da3b5pawj",
		"msgContent" : "开关打开",
		"msgType" : 4,
		"time" : 1511420861,
    "icon" : "https://images.tuyacn.com/smart/product_icon/kt.png",
		"dateTime" : "2017-11-23 15:07",
    "id" : 2967004,
    "msgSrcId" : "6c659ecc586a7da3b5pawj",
		"msgTypeContent" : "告警测试"
} ],
	"totalCount" : 2
},
	"t" : 1539243923807,
	"success" : true,
	"status" : "ok"
}
```

返回数据已经封装在CameraMessageBean中，格式如下：

| 参数           | 说明                                       |
| -------------- | ------------------------------------------ |
| dateTime       | 发生日期                                   |
| msgTypeContent | 无需关注                                   |
| msgType        | 无需关注                                   |
| time           | 发生事件的时间戳                           |
| attachPics     | 消息事件的图片，数组格式，取第一个数据     |
| attachVideos   | 消息事件的视频数据，数组格式，取第一个数据 |
| actionURL      | /                                          |
| id             | 事件ID                                     |
| msgSrcId       | 消息ID                                     |



### deleteAlarmDetectionMessageList

消息删除接口

```java
void deleteAlarmDetectionMessageList(String ids, ResultListener<Boolean> listener)
```

#### 参数说明

| 参数名称 | 类型   | 出现要求 | 描述                          |
| :------: | ------ | -------- | ----------------------------- |
|   ids    | String | 必须     | 字符串，多个消息id用逗号,分隔 |

其**整体**包含在 **json** 字段中,例如：

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

返回数据结构示例:(具体以实际请求为准)

```json
*** JSON示例 ***
{
	"result" : true,
	"t" : 1548058543873,
	"success" : true,
	"status" : "ok"
}
```



