# 摄像头配网

摄像头配网功能分为三种：EZ、AP、Qrcode。

其中前两种配网方式可以查看[**公版的SDK文档配网**](https://mimimumu.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/Activator_wifi.html), 这里主要讲解一下Qrcode的配网文档。



首先确定您已经接入了[涂鸦全屋智能SDK](https://github.com/TuyaInc/tuyasmart_home_android_sdk)。

### 获取当前的Wifi SSID

```java
WiFiUtil.getCurrentSSID()
```

### 获取配网Token

Token的有效期为10分钟，且配置成功后就会失效（再次配网需要重新获取）,获取方法如下：

```java//需要传入当前家庭的homeid
TuyaHomeSdk.getActivatorInstance().getActivatorToken (homeid, new ITuyaActivatorGetToken() {
            @Override
            public void onSuccess(String token) {
                //获取token成功
            }

            @Override
            public void onFailure(String s, String s1) {
                //获取token失败
            }
        })
```

### TuyaCameraActivatorBuilder

通过创建TuyaCameraActivatorBuilder对象来存储二维码url信息，及接口回调。方法如下：

```java
TuyaCameraActivatorBuilder builder = new TuyaCameraActivatorBuilder()
    .setToken(token)
    .setPassword(password) //传入wifi的password
    .setSsid(ssid)
    .setListener(listener); //传入ITuyaSmartCameraActivatorListener 监听对象
```

### ITuyaCameraDevActivator

配网接口实现

```java
ITuyaCameraDevActivator mTuyaActivator = TuyaHomeSdk.getActivatorInstance().newCameraDevActivator(builder);
```

### 通过二维码接口获取二维码url信息

```java
mTuyaActivator.createQRCode();
```

### 开始配网

使用上面生成的url生成二维码，确定设备处于配网状态，将生成的二维码对准摄像头，摄像头设备捕捉到二维码信息后会发出提示音或者灯光变化。此时通过以下接口开始监听配网结果。方法如下：

```java
//开始配置
mTuyaActivator.start();
```

### 停止配网

方法如下：

```java
//停止配置
mTuyaActivator.stop();
```

### 配网结果回调

配网结果通过ITuyaSmartCameraActivatorListener 配网回调接口，方法如下：

```java
//回调接口
ITuyaSmartCameraActivatorListener() {

    /**
     * 二维码生成成功
     *
     * @param qrcodeUrl 二维码URL
     */
    void onQRCodeSuccess(String qrcodeUrl);
    /**
     * 错误信息
     *
     * @param errorCode 错误码
     * @param errorMsg  错误信息
     */
    void onError(String errorCode, String errorMsg);
    /**
     * 激活成功
     *
     * @param devResp 设备ID
     */
    void onActiveSuccess(DeviceBean devResp);
};

```

### 回调销毁

```java
//回调销毁
mTuyaActivator.onDestroy();
```

### 具体配网参考流程

```java
//初始化回调接口
ITuyaSmartCameraActivatorListener mListener = new ITuyaSmartCameraActivatorListener() {
    /**
     * 二维码生成成功
     *
     * @param qrcodeUrl 二维码URL
     */
    public void onQRCodeSuccess(String qrcodeUrl){
        //使用url生成二维码
    }
    /**
     * 错误信息
     *
     * @param errorCode 错误码
     * @param errorMsg  错误信息
     */
    public void onError(String errorCode, String errorMsg){
        //配网失败的原因及错误列表，具体参考AP、EZ配网
    }
    /**
     * 激活成功
     *
     * @param devResp 设备ID
     */
    public void onActiveSuccess(DeviceBean devResp){
        //配网成功后续操作
        
    }
};
TuyaCameraActivatorBuilder builder = new TuyaCameraActivatorBuilder()
    .setToken(token)
    .setPassword(password) //传入wifi的password
    .setSsid(ssid)
    .setListener(listener); //传入ITuyaSmartCameraActivatorListener 监听对象

ITuyaCameraDevActivator mTuyaActivator =    TuyaHomeSdk.getActivatorInstance().newCameraDevActivator(builder);
//生成二维码url
mTuyaActivator.createQRCode();
//开始配置
mTuyaActivator.start();
//停止配置
mTuyaActivator.stop();
//回调销毁
mTuyaActivator.onDestroy();
```