# 用户管理



涂鸦云支持多种用户体系：手机、邮箱、UID 。其中手机支持验证码登录和密码登录两种方式，UID 登录主要用于已经有自己的账号体系的时候使用。具体内容可以参考文档 [用户管理](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/User.html)。



## 云云对接

如果使用云云对接方案，接入方已经有自己的账号体系了，就需要接入方的云端开发将自己的账号体系映射到涂鸦的账号体系，每个账号在涂鸦云端注册一个 UID，App 使用这个 UID 登录涂鸦云，以获取涂鸦设备管理权限。具体内容可以参考文档 [云云对接-用户管理](https://docs.tuya.com/zh/iot/open-api/api-list/api/user-management)。