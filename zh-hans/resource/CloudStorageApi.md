#### 本文档为云存储sdk 的操作接口说明，给对接的开发人员提供参考。
#### 集成准备 
集成[tuyasmart_camera_android_sdk](https://github.com/TuyaInc/tuyasmart_camera_android_sdk/tree/release/v3.11.1r119) 3.11.1r119 的基础上

```
    1.根目录添加仓库地址：
    ...
    maven { url 'https://jitpack.io' }
    ...
    2.模块下添加依赖：
    ...
    implementation 'com.tuya.smart:tuyasmart-ipc-camera-middleware:3.11.1r119'
    ...
    implementation 'com.tuya.smart:tuyasmart-tuyaHybridContainer:1.0.0'
    implementation 'com.github.wendux:DSBridge-Android:3.0-SNAPSHOT'
    ...
```


#### 1.CameraCloudSDK          
```
获取云存储相关的api
```
- 获取云存储 当前状态值（有无购买等）
```
    /**
     * 获取当前云存储状态值
     *
     * @param deviceBean 设备信息bean
     * @param callback  回调
     */
    getCameraCloudInfo(DeviceBean deviceBean, ICloudCacheManagerCallback callback)
```
- 获取设备云存储相关的数据,Secret，Auth等参数
```
    /**
     * 获取设备云存储相关的数据,Secret，Auth等参数
     *
     * @param devId    设备id
     * @param timeZone 时区
     * @param callback
     */
    public void getCloudMediaCount(String devId, String timeZone, ICloudCacheManagerCallback callback)
```
- 获取指定时间的时间片

```
    /**
     * 获取指定时间的时间片
     *
     * @param devId  设备id
     * @param timeGT 开始时间（13位时间戳）
     * @param timeLT 结束时间
     */
    public void getTimeLineInfoByTimeSlice(String devId, String timeGT, String timeLT, ICloudCacheManagerCallback callback)
```
- 根据时间片段始末获取相应的移动侦测数据

```
    /**
     * 根据时间片段始末获取相应的移动侦测数据
     *
     * @param devId  设备id
     * @param timeGT 开始时间
     * @param timeLT 结束时间
     * @param offset 第几页，默认0
     * @param limit  每次拉取条数，默认-1，表示所有数据
     */
    public void getMotionDetectionByTimeSlice(String devId, String timeGT, String timeLT, int offset, int limit, ICloudCacheManagerCallback callback) 
```
- 云存储 购买

```
    /**
     * 获取云存储 购买
     *
     * @param mContext
     * @param deviceBean    设备id
     * @param homeId   家庭id
     */
    public void buyCloudStorage(Context mContext, DeviceBean deviceBean, String homeId) {
        CameraCloudManager.getInstance().getCloudStorageUrl(mContext, deviceBean, homeId);
    }
```
- API回收

```
    /**
     * 退出的时候调用
     */
    public void onDestroy()
```

#### 2.ICloudCacheManagerCallback

```
调用sdk之后的回调方法
```

```
    /**
     * 返回有云存储数据日期
     *
     * @param cloudDayBeanList
     */
    void getCloudDayList(List<CloudDayBean> cloudDayBeanList);

    /**
     * 返回 云存储 encryKey
     *
     * @param encryKey
     */
    void getCloudSecret(String encryKey);

    /**
     * 返回 云存储 authorityJson
     *
     * @param authorityJson
     */
    void getAuthorityGet(String authorityJson);

    /**
     * 返回 云存储 根据某一天时间段查询的数据
     *
     * @param timePieceBeans
     */
    void getTimePieceInfoByTimeSlice(List<TimePieceBean> timePieceBeans);

    /**
     * 返回 云存储-移动侦测 根据某一天时间段查询的数据
     *
     * @param timeRangeBeans
     */
    void getMotionDetectionByTimeSlice(List<TimeRangeBean> timeRangeBeans);

    /**
     * 返回 云存储 状态
     *
     * @param code
     */
    void getCloudStatusSuccess(int code);

    /**
     * 返回 云存储 配置信息，需要传入sdk验证
     *
     * @param config
     */
    void getCloudConfigDataTags(String config);
    
    /**
     * 错误码
     *
     * @param errorCode
     */
    void onError(int errorCode);

```

#### 3.状态值及错误码
- 状态值
```
    //未开通服务
    public static final int NO_SERVES = 10001;
    //已开通服务，无数据
    public static final int SERVES_NO_CLOUD_DATA = 10002;
    //已开通服务，有数据
    public static final int SERVES_DATA = 10003;
    //服务已过期，有数据
    public static final int EXPIRED_SERVES_DATA = 10004;
    //服务已过期，无数据
    public static final int EXPIRED_SERVES_NO_CLOUD_DATA = 10005;
```
- 错误码

```
    //获取SECRET 失败
    public static final int ERROR_QUERY_SECRET_CODE = 10100;
    //获取AUTH 失败
    public static final int ERROR_QUERY_AUTH_CODE = 10101;
    //其他异常
    public static final int ERROR_QUERY_CODE = 10110;
    
    //获取认证信息失败
    public static final int ERROR_GET_CLOUD_DAY_COUNT = 10010;
    //获取某天的时间片（鉴权信息）失败
    public static final int ERROR_GET_MEDIA_PREFIXS = 10011;
```
---------------------------------------------------

##### 4.ITYCloudCamera

```
云存储播放SDK
```
- 初始化，创建设备对象

```
     /**
     * 初始化，创建云存储播放设备对象
     *
     * @param cachePath 缓存路径
     * @param devId     设备id
     */
    void createCloudDevice(String cachePath, String devId)
```
- 注册/反注册p2p监听

```
     /**
     * 注册p2p 连接监听
     *
     * @param listener
     */
    void registorOnP2PCameraListener(OnP2PCameraListener listener);

    /**
     * 返注册p2p 连接监听
     */
    void removeOnP2PCameraListener();
```
- 关联播放器与播放组件view

```
    /**
     * 关联view
     * @param view
     */
    void generateCloudCameraView(IRegistorIOTCListener view);

```
- 配置云存储数据tags(开始播放云存前需要先进行配置)

```
     /**
     * 配置云存储数据tags(开始播放云存前需要先进行配置)
     * @param jsonStr
     * @param callBack
     * @return
     */
    int configCloudDataTags(String jsonStr, OperationDelegateCallBack callBack);
```
- 播放云存储相关

```
     /**
     * 云存储播放
     *
     * @param mStartTime           开始播放时间 unix秒时间戳
     * @param mEndTime             结束播放时间 unix秒时间戳
     * @param isEvent              是否是事件
     * @param jsonAuthParams       鉴权信息
     * @param encryptKey           key
     * @param callback             播放成功或失败
     * @param playFinishedCallBack 播放结束时回调
     */
    void playCloudDataWithStartTime(long mStartTime, long mEndTime, boolean isEvent, String jsonAuthParams, String encryptKey, OperationCallBack callback, OperationCallBack playFinishedCallBack);
    /**
     * 播放暂停
     * @param callBack
     * @return
     */
    int pausePlayCloudVideo(OperationDelegateCallBack callBack);

    /**
     * 播放继续
     * @param callBack
     * @return
     */
    int resumePlayCloudVideo(OperationDelegateCallBack callBack);

    /**
     * 停止播放
     * @param callBack
     * @return
     */
    int stopPlayCloudVideo(OperationDelegateCallBack callBack);
```
- 声音（是否静音）

```
    /**
     * 获取声音状态
     * @return
     */
    int getCloudMute();

    /**
     * 设置声音状态
     * @param mute
     * @param callBack
     */
    void setCloudMute(final int mute, OperationDelegateCallBack callBack);
```
- 截图

```
    /**
     * 截图
     * @param absoluteFilePath 路径
     * @param callBack
     * @return
     */
    int snapshot(String absoluteFilePath, OperationDelegateCallBack callBack);
```
- 录制

```
    /**
     * 录制
     * @param folderPath    保存路径
     * @param fileName  保存文件名
     * @param callBack
     * @return
     */
    int startRecordLocalMp4(String folderPath, String fileName, OperationDelegateCallBack callBack);

    /**
     * 停止录制
     * @param callBack
     * @return
     */
    int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```
- 反初始化

```
    /**
     * 反初始化
     */
    void deinitCloudCamera();

```