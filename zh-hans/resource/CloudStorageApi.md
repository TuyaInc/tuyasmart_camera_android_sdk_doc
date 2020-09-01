# 云存储数据获取 API




## 集成准备 

项目模块的 build.gradle：

```groovy
...
  implementation 'com.tuya.smart:tuyasmart-ipc-camera-middleware:3.14.3r133'
...
```



## 获取云存储数据接口方法

**CameraCloudSDK ** 是云存储数据的 API 接口调用的实体对象，提供了获取云存储购买，时间数据相关的 API 方法。

### 获取云存储 当前状态值（有无购买等）

```java
void getCameraCloudInfo(DeviceBean deviceBean, ICloudCacheManagerCallback callback)
```
**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| deviceBean | 设备信息 |
| callback | 回调方法|

**示例代码**

```java
cameraCloudSDK.getCameraCloudInfo(TuyaHomeSdk.getDataInstance().getDeviceBean(devId), CameraCloudStorageActivity.this);
```



### 获取设备云存储相关的数据 Secret、Auth 等参数

```java
public void getCloudMediaCount(String devId, String timeZone, ICloudCacheManagerCallback callback)
```
**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| devId | 设备id |
| timeZone | 时区|
| callback | 回调方法|

**示例代码**

```java
cameraCloudSDK.getCloudMediaCount(devId, TimeZone.getDefault().getID(), CameraCloudStorageActivity.this);
```



### 获取指定时间的时间片

```java
public void getTimeLineInfoByTimeSlice(String devId, String timeGT, String timeLT, ICloudCacheManagerCallback callback)
```
**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| devId | 设备id |
| timeGT | 开始时间（13位时间戳）|
| timeLT | 结束时间|
| callback | 回调方法|

**示例代码**

```java
getTimeLineInfoByTimeSlice(devId, String.valueOf(dayBean.getCurrentStartDayTime()), String.valueOf(dayBean.getCurrentDayEndTime()));
```



### 根据时间片段始末获取相应的移动侦测数据

```java
public void getMotionDetectionByTimeSlice(String devId, String timeGT, String timeLT, int offset, int limit, ICloudCacheManagerCallback callback) 
```

**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| devId | 设备id |
| timeGT | 开始时间（13位时间戳）|
| timeLT | 结束时间|
| offset | 第几页，默认0|
| limit | 每次拉取条数，默认-1，表示所有数据|
| callback | 回调方法|

**示例代码**

```java
cameraCloudSDK.getMotionDetectionByTimeSlice(devId, timeGT, timeLT, offset, limit, this);
```



### 云存储购买地址接口

```java
public void buyCloudStorage(Context mContext, DeviceBean deviceBean, String homeId, ICloudManagerCallback callback) {
        CameraCloudManager.getInstance().getCloudStorageUrl(mContext, deviceBean, homeId);
    }
```

**参数说明**

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| mContext | 上下文 |
| deviceBean | 设备信息）|
| homeId | 家庭id|
| callback | 回调方法|

**示例代码**

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



### 销毁

```java
public void onDestroy()
```

**示例代码**

```java
if (null != cameraCloudSDK) {
  cameraCloudSDK.onDestroy();
}
```



### ICloudCacheManagerCallback

**描述**

调用 sdk 之后的回调方法



#### 返回有云存储数据日期

```java
void getCloudDayList(List<CloudDayBean> cloudDayBeanList);
```
##### 参数说明

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| cloudDayBeanList | 云存储日期数据集 |



#### 返回云存储 encryKey


```java
void getCloudSecret(String encryKey);
```
##### 参数说明

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| encryKey | 云存储秘钥 |



#### 返回云存储 authorityJson


```java
void getAuthorityGet(String authorityJson);
```
##### 参数说明

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| authorityJson | 校验数据 |



#### 返回云存储根据某一天时间段查询的数据


```java
void getTimePieceInfoByTimeSlice(List<TimePieceBean> timePieceBeans);
```
##### 参数说明

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| timePieceBeans | 时间片段数据集 |



#### 返回云存储移动侦测根据某一天时间段查询的数据


```java
void getMotionDetectionByTimeSlice(List<TimeRangeBean> timeRangeBeans);
```
##### 参数说明

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| timeRangeBeans | 移动侦测时间片段数据集 |



#### 返回云存储状态


```java
void getCloudStatusSuccess(int code);
```
##### 参数说明

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| code | 购买状态 |
> code值，参考文章底下的状态码



#### 返回云存储配置信息，需要传入 sdk 验证


```java
void getCloudConfigDataTags(String config);
```
##### 参数说明

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| config | 配置信息数据 |



#### 错误回调


```java
void onError(int errorCode);
```
##### 参数说明

| 参数    | 说明                               |
| :------- | :---------------------------------- |
| errorCode | 错误码 |



#### 状态值

| 状态值    | 说明                               |
| :------- | :---------------------------------- |
| 10001 | 未开通服务 |
| 10002 | 已开通服务，无数据 |
| 10003 | 已开通服务，有数据 |
| 10004 | 服务已过期，有数据 |
| 10005 | 服务已过期，无数据 |



#### 错误码

| 错误码    | 说明                               |
| :------- | :---------------------------------- |
| 10100 | 获取 SECRET 失败 |
| 10101 | 获取 AUTH 失败 |
| 10110 | 其他异常 |
| 10010 | 获取认证信息失败 |
| 10011 | 获取某天的时间片（鉴权信息）失败 |

