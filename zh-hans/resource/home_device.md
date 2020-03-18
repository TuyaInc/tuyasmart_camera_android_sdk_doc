# 家庭与设备管理



## 家庭管理

用户登录成功后需要通过`TuyaHomeSdk.getHomeManagerInstance()`去获取整个家庭列表的信息,然后初始化其中的一个家庭`ITuyaHomeManager`，获取家庭详情信息，对家庭中的设备进行管理，控制。参考文档[家庭管理](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/HomeManager.html)。



## 设备管理

涂鸦智能提供了丰富的接口供开发者实现设备信息的获取和管理能力(移除等)。设备相关的返回数据都采用异步消息的方式通知接受者。参考文档[设备管理](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/Device_standard.html)。

#####  【注意事项】

- 设备控制必须先初始化数据，即先调用`TuyaHomeSdk.newHomeInstance(homeId).getHomeDetail(ITuyaHomeResultCallback callback)`
- 设备控制如果需要使用经纬度，需要再配网前调用`TuyaSdk.setLatAndLong`，其中 `lon` , `lat`  用来标示经纬度信息。

