# TuyaSmartCameraP2P

Implementation class for getting IPC Config data

## Get IPC Config data method

```java
public void requestCameraInfo(final String devId, final ICameraConfig callback)
```

**Parameter Description**

| Parameter              |      Description      |
| :-------------------- | :------------ |
| devId  | device Id |
| callback | data callback |

#### ICameraConfig callback interface class

| Callback      | Description         |
| --------- | ------------ |
| onSuccess | request successful callback |
| onFailure | request failure callback |

Request successful callback（onSuccess）

```java
public void onSuccess(BusinessResponse var1, ConfigCameraBean var2, String var3);
```

Request failure callback（onFailure）

```java
public void onFailure(BusinessResponse var1, ConfigCameraBean var2, String var3);
```

**ConfigCameraBean model class**

this model is required when creating a device, see details: **ICameraP2P-create device**。

| Name                     | Description   |
| :------------------------ | :------ |
| password                   | password   |
| p2pId               | p2p Id |
| p2pType  | p2p type  |
| ...          | ... |

**sample code**

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



### Cancel all IPC Config data requests

```java
public void destroyCameraBusiness();
```

