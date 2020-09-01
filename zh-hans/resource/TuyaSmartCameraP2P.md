# TuyaSmartCameraP2P

获取 IPC Config 数据的实现类

## 获取 IPC Config 数据

```java
public void requestCameraInfo(final String devId, final ICameraConfig callback)
```

**参数说明**

| 参数              |      描述      |
| :-------------------- | :------------ |
| devId  | 设备 Id |
| callback | 数据回调 |

#### ICameraConfig 回调接口类

| 回调      | 说明         |
| --------- | ------------ |
| onSuccess | 请求成功回调 |
| onFailure | 请求失败回调 |

请求成功回调（onSuccess）

```java
public void onSuccess(BusinessResponse var1, ConfigCameraBean var2, String var3);
```

请求失败回调（onFailure）

```java
public void onFailure(BusinessResponse var1, ConfigCameraBean var2, String var3);
```

**ConfigCameraBean 数据模型**

该模型需要在创建设备时传入，详见 **ICameraP2P-创建设备**。

| 名称                     | 描述   |
| :------------------------ | :------ |
| password                   | 秘钥   |
| p2pId               | p2p Id |
| p2pType  | p2p 的类型  |
| ...          | ... |

**示例代码**

```java
private void getApi() {
	mSmartCameraP2P = new TuyaSmartCameraP2P();
	mSmartCameraP2P.requestCameraInfo(devId, new ICameraConfig() {
        @Override
        public void onFailure(BusinessResponse var1, ConfigCameraBean var2, String var3) {
            ToastUtil.shortToast(CameraPanelActivity.this, "get cameraInfo failed");
        }

        @Override
        public void onSuccess(BusinessResponse var1, ConfigCameraBean var2, String var3) {
            p2pWd = var2.getPassword();
            p2pId = var2.getP2pId();
            initCameraView(var2);
        }
    });
}	
```



### 取消所有 IPC Config 数据请求

```java
public void destroyCameraBusiness();
```

