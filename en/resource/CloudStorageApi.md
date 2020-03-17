# Cloud storage data API



## Integration preparation 

Build.gradle of the project module:

```groovy
...
    implementation 'com.tuya.smart:tuyasmart-ipc-camera-middleware:3.14.3r133'
...
```



## Get cloud storage data interface method

CameraCloudSDK is an entity object called by the API interface of cloud storage ATOP, and provides API methods related to obtaining cloud storage purchase and time data.



### getCameraCloudInfo

Get cloud storage current status value (with or without purchase, etc.).

```java
void getCameraCloudInfo(DeviceBean deviceBean, ICloudCacheManagerCallback callback)
```

**Parameter Description**

| Parameter  | Description |
| ---------- | ----------- |
| deviceBean | device info |
| callback   | Callback    |

**Sample Code**

```java
cameraCloudSDK.getCameraCloudInfo(TuyaHomeSdk.getDataInstance().getDeviceBean(devId), CameraCloudStorageActivity.this);
```



### getCloudMediaCount

Get device cloud storage related data, Secret, Auth and other parameters.

```java
public void getCloudMediaCount(String devId, String timeZone, ICloudCacheManagerCallback callback)
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| devId     | device Id   |
| timeZone  | time Zone   |
| callback  | Callback    |

**Sample Code**

```java
cameraCloudSDK.getCloudMediaCount(devId, TimeZone.getDefault().getID(), CameraCloudStorageActivity.this);
```



### getTimeLineInfoByTimeSlice

Get time slice at specified time.

```java
public void getTimeLineInfoByTimeSlice(String devId, String timeGT, String timeLT, ICloudCacheManagerCallback callback)
```

**Parameter Description**

| Parameter | Description                   |
| --------- | ----------------------------- |
| devId     | device Id                     |
| timeGT    | Start time (13-bit timestamp) |
| timeLT    | End Time                      |
| callback  | Callback                      |

**Sample Code**

```java
getTimeLineInfoByTimeSlice(devId, String.valueOf(dayBean.getCurrentStartDayTime()), String.valueOf(dayBean.getCurrentDayEndTime()));
```



### getMotionDetectionByTimeSlice

Get the corresponding motion detection data according to the beginning and end of the time segment.

```java
public void getMotionDetectionByTimeSlice(String devId, String timeGT, String timeLT, int offset, int limit, ICloudCacheManagerCallback callback) 
```

**Parameter Description**

| Parameter | Description                                                 |
| --------- | ----------------------------------------------------------- |
| devId     | device Id                                                   |
| timeGT    | Start time (13-bit timestamp)                               |
| timeLT    | End Time                                                    |
| offset    | Page number, default 0                                      |
| limit     | Number of pulls at a time, default -1, which means all data |
| callback  | callback                                                    |

**Sample Code**

```java
cameraCloudSDK.getMotionDetectionByTimeSlice(devId, timeGT, timeLT, offset, limit, this);
```



### buyCloudStorage

Cloud storage purchase address interface

```java
public void buyCloudStorage(Context mContext, DeviceBean deviceBean, String homeId, ICloudManagerCallback callback) {
  CameraCloudManager.getInstance().getCloudStorageUrl(mContext, deviceBean, homeId);
}
```

**Parameter Description**

| Parameter  | Description |
| ---------- | ----------- |
| mContext   | /           |
| deviceBean | device Info |
| homeId     | home Id     |
| callback   | callback    |

**Sample Code**

```java
cameraCloudSDK.buyCloudStorage(CameraCloudStorageActivity.this,
                               TuyaHomeSdk.getDataInstance().getDeviceBean(devId),
                               String.valueOf(FamilyManager.getInstance().getCurrentHomeId()), new ICloudManagerCallback() {
                                 @Override
                                 public void onError(int i) {

                                 }

                                 @Override
                                 public void onSuccess(Object o) {
                                   String uri = (String) o;
                                   Intent intent = new Intent(CameraCloudStorageActivity.this, WebViewActivity.class);
                                   intent.putExtra("Uri",uri);
                                   startActivity(intent);
                                 }
                               });
```



### Destroy

```java
public void onDestroy()
```

**Sample Code**

```java
if (null != cameraCloudSDK) {
  cameraCloudSDK.onDestroy();
}
```





### ICloudCacheManagerCallback

**description**

Callback method after calling SDK



#### getCloudDayList

Returns date with cloud storage data.

```java
void getCloudDayList(List<CloudDayBean> cloudDayBeanList);
```

**Parameter Description**

| Parameter        | Description             |
| ---------------- | ----------------------- |
| cloudDayBeanList | Cloud storage date List |



#### getCloudSecret

```java
void getCloudSecret(String encryKey);
```

**Parameter Description**

| Parameter | Description       |
| --------- | ----------------- |
| encryKey  | Cloud storage key |



#### getAuthorityGet

Cloud storage authorityJson

```java
void getAuthorityGet(String authorityJson);
```

**Parameter Description**

| Parameter     | Description |
| ------------- | ----------- |
| authorityJson | Check data  |



#### getTimePieceInfoByTimeSlice

Returns data queried by cloud storage based on a certain time period.

```java
void getTimePieceInfoByTimeSlice(List<TimePieceBean> timePieceBeans);
```

**Parameter Description**

| Parameter      | Description          |
| -------------- | -------------------- |
| timePieceBeans | Time-slice data list |



#### getMotionDetectionByTimeSlice

Returns data from cloud storage motion detection queries based on a certain time period.

```java
void getMotionDetectionByTimeSlice(List<TimeRangeBean> timeRangeBeans);
```

**Parameter Description**

| Parameter      | Description                             |
| -------------- | --------------------------------------- |
| timeRangeBeans | Motion detection time segment data list |



#### getCloudStatusSuccess

Back to cloud storage status.

```java
void getCloudStatusSuccess(int code);
```

**Parameter Description**

| Parameter | Description       |
| --------- | ----------------- |
| code      | Cloud status code |

> code value, refer to the status code at the bottom of the article.



#### getCloudConfigDataTags

Return cloud storage configuration information, need to pass in SDK verification.

```java
void getCloudConfigDataTags(String config);
```

**Parameter Description**

| Parameter | Description                    |
| --------- | ------------------------------ |
| config    | Configuration information data |



#### onError

Error callback

```java
void onError(int errorCode);
```

**Parameter Description**

| Parameter | Description |
| --------- | ----------- |
| errorCode | Error code  |



#### Status value

| Status value | Description                             |
| ------------ | --------------------------------------- |
| 10001        | Not Opened                              |
| 10002        | Service opened, no data                 |
| 10003        | Service is available, data is available |
| 10004        | Service expired with data               |
| 10005        | Service expired, no data                |



#### Error code

| error code | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| 10100      | Get SECRET failed                                            |
| 10101      | Failed to get AUTH                                           |
| 10110      | Other exceptions                                             |
| 10010      | Failed to obtain authentication information                  |
| 10011      | Failed to get time slice (authentication information) for a certain day |

