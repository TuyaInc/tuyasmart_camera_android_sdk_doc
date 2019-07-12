# 获取相关参数

> p2pID，pwd, localkey,initStr,p2pkey这几个参数是TuyaCameraSDK初始化所必须有的，这几个参数没有对应成功，p2p的构建通道就不可能成功。

获取p2pID，pwd, localkey,initStr,p2pkey需要接入涂鸦公版SDK提供的api接口及能力。



#### 涂鸦公版SDK介绍

涂鸦公版的SDK封装涂鸦云HTTP API接口封装，因此涂鸦智能的摄像机SDK获取的API需要用到涂鸦公版的SDK的方法，具体了解请查看[TUYA公版SDK的获取方式及接入指南](https://mimimumu.github.io/tuyasmart_home_android_sdk_doc/zh-hans/)。



#### 调用方法：

接入涂鸦全屋智能Android SDK，并初始化其SDK后，需要使用其通用的接口调用方法来获取摄像头配置参数。

```java 
	TuyaHomeSdk.getRequestInstance().requestWithApiName(String apiName, String version, Map postData, final IRequestCallback callback)
```

获取p2pid、pwd，localkey,initStr,p2pkey需要调用的api接口：

- 接口说明 

  | api name              | version |      描述      |
  | -------------------- | ------------ | --------------- |
  | tuya.m.ipc.config.get |   “2.0” | 摄像头配置信息 |
  
- 请求参数

  | 名称  | 类型   | 描述               |
  | ----- | ------ | ------------------ |
  | devId | 字符串 | 摄像头对应的设备id |
  

  ```java
  postData:(“devId”,devId)
  ```

- 响应

  | 名称                     | 类型   | 描述                          |
  | ------------------------ | ------ | ----------------------------- |
  | result                   | 字典   | 返回结果                      |
  | result.id                | 字符串 | 设备id                        |
  | result.p2pConfig         | 字典   | 扩展配置                      |
  | result.p2pId             | 字符串 | p2p连接id                     |
  | result.password          | 字符串 | p2p连接密码                   |
  | result.timeZoneId        | 字符串 | 设备所在时区                  |
  | status                   | 字符串 | 状态（成功：ok；失败：error） |
  | success                  | 布尔值 | 是否成功                      |
  | result.p2pConfig.initStr | 字符串 | initStr                       |
  | result.p2pConfig.p2pKey  | 字符串 | p2pKey                        |
  

  ```json
  {
  	result = 	{
  		password = "xxxxxx",
  		id = "xxxxxxxxxxxxxxxxxx",
  		p2pConfig = 	{
       		initStr= "xxxxxxxxxxxxxxxxxx",
              p2pKey= "xxxxxxxxxxxxxxxxxx"
  		},
  		p2pId = "XXXXXXXXXXXXXXXXXXX",
  		timeZoneId = "Asia/Shanghai",
  	},
  	success = 1,
  	status = "ok",
  }
  ```
