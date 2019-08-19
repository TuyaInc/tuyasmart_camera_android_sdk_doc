#### This document explains the operation interface of cloud storage SDK and provides reference for developers.
#### Integration preparation 
[tuyasmart_camera_android_sdk](https://github.com/TuyaInc/tuyasmart_camera_android_sdk/tree/release/v3.11.1r119) 3.11.1r119 must be integrated first

```
    1.add the address to root repositories：
    ...
    maven { url 'https://jitpack.io' }
    ...
    2.adding dependencies under app module：
    ...
    implementation 'com.tuya.smart:tuyasmart-ipc-camera-middleware:3.11.1r119'
    ...
    implementation 'com.tuya.smart:tuyasmart-tuyaHybridContainer:1.0.0'
    implementation 'com.github.wendux:DSBridge-Android:3.0-SNAPSHOT'
    ...
```


#### 1.CameraCloudSDK          
```
Capturing APIs related to cloud storage
```
- Get the current status value of cloud storage (with or without purchase, etc.)
```
    /**
     * the current status value of cloud storage
     *
     * @param deviceBean    device info
     * @param callback  
     */
    getCameraCloudInfo(DeviceBean deviceBean, ICloudCacheManagerCallback callback)
```
- Get cloud storage related data, Secret, Auth and other parameters
```
    /**
     * Get cloud storage related data, Secret, Auth and other parameters
     *
     * @param devId    
     * @param timeZone 
     * @param callback
     */
    public void getCloudMediaCount(String devId, String timeZone, ICloudCacheManagerCallback callback)
```
- Get the time slice for the specified time

```
    /**
     * Get the time slice for the specified time
     *
     * @param devId  
     * @param timeGT Start time (13 bit timestamp)
     * @param timeLT End time (13 bit timestamp)
     */
    public void getTimeLineInfoByTimeSlice(String devId, String timeGT, String timeLT, ICloudCacheManagerCallback callback)
```
- Acquisition of corresponding mobile detection data based on the beginning and end of time segment

```
    /**
     * Acquisition of corresponding mobile detection data based on the beginning and end of time segment
     *
     * @param devId  
     * @param timeGT Start time (13 bit timestamp)
     * @param timeLT End time (13 bit timestamp)
     * @param offset default 0
     * @param limit  Limitation of number(default - 1,representing all data)
     */
    public void getMotionDetectionByTimeSlice(String devId, String timeGT, String timeLT, int offset, int limit, ICloudCacheManagerCallback callback) 
```
- Purchase Cloud Storage 

```
    /**
     * Purchase
     *
     * @param mContext
     * @param deviceBean    
     * @param homeId   
     */
    public void buyCloudStorage(Context mContext, DeviceBean deviceBean, String homeId) {
        CameraCloudManager.getInstance().getCloudStorageUrl(mContext, deviceBean, homeId);
    }
```
- destroy

```
    /**
     * Called on exit
     */
    public void onDestroy()
```

#### 2.ICloudCacheManagerCallback

```
Callback method after calling SDK
```

```
    /**
     * Return the date of cloud storage data
     *
     * @param cloudDayBeanList
     */
    void getCloudDayList(List<CloudDayBean> cloudDayBeanList);

    /**
     * Return the encryKey of cloud storage data 
     *
     * @param encryKey
     */
    void getCloudSecret(String encryKey);

    /**
     * Return the authorityJson of cloud storage data  
     *
     * @param authorityJson
     */
    void getAuthorityGet(String authorityJson);

    /**
     * Return cloud storage data queried according to a certain period of time
     *
     * @param timePieceBeans
     */
    void getTimePieceInfoByTimeSlice(List<TimePieceBean> timePieceBeans);

    /**
     * Return cloud storage motion detection data queried according to a certain period of time
     *
     * @param timeRangeBeans
     */
    void getMotionDetectionByTimeSlice(List<TimeRangeBean> timeRangeBeans);

    /**
     * Return cloud storage status
     *
     * @param code
     */
    void getCloudStatusSuccess(int code);

    /**
     * Return cloud storage config info，for the SDK validation
     *
     * @param config
     */
    void getCloudConfigDataTags(String config);
    
    /**
     * @param errorCode
     */
    void onError(int errorCode);

```

#### 3.State values and error codes
- State values
```
    //Unopened Services
    public static final int NO_SERVES = 10001;
    //Services have been opened, but no data
    public static final int SERVES_NO_CLOUD_DATA = 10002;
    //Services have been opened, data available
    public static final int SERVES_DATA = 10003;
    //service has expired and data is available
    public static final int EXPIRED_SERVES_DATA = 10004;
    //Service has expired without data
    public static final int EXPIRED_SERVES_NO_CLOUD_DATA = 10005;
```
- Error codes

```
    //get SECRET failed
    public static final int ERROR_QUERY_SECRET_CODE = 10100;
    //get AUTH failed
    public static final int ERROR_QUERY_AUTH_CODE = 10101;
    //other exception
    public static final int ERROR_QUERY_CODE = 10110;
    
    //get auth info failed
    public static final int ERROR_GET_CLOUD_DAY_COUNT = 10010;
    //Failure to obtain time slice of a day (authentication information)
    public static final int ERROR_GET_MEDIA_PREFIXS = 10011;
```
---------------------------------------------------

##### 4.ITYCloudCamera

```
Cloud Storage Plays SDK
```
- Initialize, create device object

```
     /**
     * Initialize, create device object
     *
     * @param cachePath 
     * @param devId     
     */
    void createCloudDevice(String cachePath, String devId)
```
- Registration/anti-registration P2P monitoring

```
     /**
     * registration P2P monitoring
     *
     * @param listener
     */
    void registorOnP2PCameraListener(OnP2PCameraListener listener);

    /**
     * anti-registration P2P monitoring
     */
    void removeOnP2PCameraListener();
```
- Associate Player with Player Component

```
    /**
     * Associate view
     * @param view
     */
    void generateCloudCameraView(IRegistorIOTCListener view);

```
- Configure cloud storage tags(must configure before start playing.)

```
     /**
     * Configure cloud storage tags
     * @param jsonStr
     * @param callBack
     * @return
     */
    int configCloudDataTags(String jsonStr, OperationDelegateCallBack callBack);
```
- Cloud Storage Play API

```
     /**
     * play
     * @param mStartTime           UNIX second timestamp
     * @param mEndTime             UNIX second timestamp
     * @param isEvent              Is it a mobile detection event?
     * @param jsonAuthParams       Authentication Information
     * @param encryptKey           
     * @param callback             
     * @param playFinishedCallBack 
     */
    void playCloudDataWithStartTime(long mStartTime, long mEndTime, boolean isEvent, String jsonAuthParams, String encryptKey, OperationCallBack callback, OperationCallBack playFinishedCallBack);
    /**
     * pause
     * @param callBack
     * @return
     */
    int pausePlayCloudVideo(OperationDelegateCallBack callBack);

    /**
     * resume
     * @param callBack
     * @return
     */
    int resumePlayCloudVideo(OperationDelegateCallBack callBack);

    /**
     * stop
     * @param callBack
     * @return
     */
    int stopPlayCloudVideo(OperationDelegateCallBack callBack);
```
- enable sound

```
    /**
     * is mute or not
     * @return
     */
    int getCloudMute();

    /**
     * set mute
     * @param mute
     * @param callBack
     */
    void setCloudMute(final int mute, OperationDelegateCallBack callBack);
```
- snapshot

```
    /**
     * 
     * @param absoluteFilePath 
     * @param callBack
     * @return
     */
    int snapshot(String absoluteFilePath, OperationDelegateCallBack callBack);
```
- record

```
    /**
     * record
     * @param folderPath    save path
     * @param fileName  save file 
     * @param callBack
     * @return
     */
    int startRecordLocalMp4(String folderPath, String fileName, OperationDelegateCallBack callBack);

    /**
     * stop
     * @param callBack
     * @return
     */
    int stopRecordLocalMp4(OperationDelegateCallBack callBack);
```
- Anti-initialization

```
    /**
     * Anti-initialization
     */
    void deinitCloudCamera();

```