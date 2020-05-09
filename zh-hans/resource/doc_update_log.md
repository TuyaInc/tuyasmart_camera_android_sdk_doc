# 版本记录




## 文档更新记录

| 版本号 | 编写/修订说明 | 修订人 | 修订时间   |
| :------ | :------------- | :------ | :---------- |
| v1.0.0 | 创建          | 黄大举 | 2020/02/29 |



## SDK 更新记录


- 2020.5.9
  - 更新 sdk（版本 3.17.0r139）
    - 升级依赖的 homeSDK ：implementation 'com.tuya.smart:tuyasmart:3.17.0-beta1'
    - 删除paho mqtt 库的依赖： - ~~implementation 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'~~
    - 获取 camera 配置信息接口 requestCameraInfo 更新
      -  注意：p2pType =2 时参数p2pId，initString ；p2pType =4 时参数 p2pId，token的获取方式
    - 修复音频问题（切换清晰度声音关闭），so崩溃问题，提升sdk稳定性；
    - ITuyaCameraDevice 增加设备所有dp点操作的上报回调 setRegisterDevListener
  - 更新优化 demo
    - 混淆修改 #mqtt
       - 移除 
                  - ~~-keep class org.eclipse.paho.client.mqttv3.** { *; }~~
                  - ~~-dontwarn org.eclipse.paho.client.mqttv3.**~~
      - 新增 
          - -keep class com.tuya.smart.mqttclient.mqttv3.** { *; }
          - -dontwarn com.tuya.smart.mqttclient.mqttv3.**
- 2020.3.31
  - 更新底层库，修复armabi消息中心视频播放问题
- 2020.3.4
  - 更新sdk（版本 3.15.0r135），消息中心多媒体预览（云存储视频播放）
- 2019.11.15
  - 更新sdk(版本 3.13.0r129)，对应的ffmpeg是4.1.4版本
  - 更新sdk demo
- 2019.10.8
  - 更新sdk（版本 3.12.6r125）
- 2019.8.1
  - 支持云存储功能 （版本 3.11.1r119）
- 2019.7.13
  - 新的sdk代码重构，接口方法有变更，为了兼容老版本sdk请使用tuyaCamera:3.11.0r119h2。建议老用户向上升级
- 2019.6.11
  - 支持arm64