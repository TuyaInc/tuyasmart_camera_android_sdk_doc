# 家庭与设备管理



## 家庭管理

用户登录成功后需要通过 `TuyaHomeSdk.getHomeManagerInstance() `去获取整个家庭列表的信息，然后初始化其中的一个家庭 `ITuyaHomeManager` ，获取家庭详情信息，对家庭中的设备进行管理，控制。参考文档 [家庭管理](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/HomeManager.html)。



## 设备管理

涂鸦智能提供了丰富的接口供开发者实现设备信息的获取和管理能力(移除等)。设备相关的返回数据都采用异步消息的方式通知接受者。参考文档 [设备管理](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/Device_standard.html)。

##### 【注意事项】

- 设备控制必须先初始化数据，即先调用 `TuyaHomeSdk.newHomeInstance(homeId).getHomeDetail(ITuyaHomeResultCallback callback)`
- 设备控制如果需要使用经纬度，需要在配网前调用 `TuyaSdk.setLatAndLong`，其中  `lon`、`lat`  用来标示经纬度信息。

## 摄像机

在获取到设备列表后，就可以根据设备的类型来判断是否是智能摄像机设备，如果是智能摄像机设备，则可以根据 DeviceBean 中的信息来创建摄像机对象。

### 判断是否是智能摄像机

可以根据 DeviceBean 的 category 属性来判断设备的类型，智能摄像机的类型是 `sp`。

**示例代码**

```java
private void gotoDeviceCommonActivity(DeviceBean devBean) {
    if ("sp".equals(devBean.getProductBean().getCategory())) {
        //是智能摄像机
    }
}
```

### 摄像机配置信息

筛选出智能摄像机设备后，就可以根据设备 id，即 DeviceBean 的 devId 属性来获取摄像机的配置信息。详见[获取设备信息](./LiveProcess.html#获取设备信息)。

### P2P 类型

涂鸦智能摄像机支持三种 p2p 通道实现方案，Camera SDK 会根据 p2p 类型来初始化不同的摄像机具体实现的对象，通过下面的方式获取设备的 p2p 类型。

```java
Map<String, Object> map = deviceBean.getSkills();
int p2pType = -1;
if (map == null || map.size() == 0) {
    p2pType = -1;
} else {
    p2pType = (Integer) (map.get("p2pType"));
}
```