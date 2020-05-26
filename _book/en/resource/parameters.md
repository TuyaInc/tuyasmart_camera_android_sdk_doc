# CameraBusiness



## Introduction

Implementation class for getting IPC Config data



### Get IPC Config data method

```java
void requestCameraInfo(String devId, ResultListener<CameraInfoBean> listener)
```

**Parameter Description**

| Parameter | Description   |
| --------- | ------------- |
| devId     | device Id     |
| listener  | data callback |

CameraInfoBean

| Name             | Description |
| ---------------- | ----------- |
| password         | password    |
| p2pId            | p2p Id      |
| p2pSpecifiedType | p2p type    |
| P2pConfig        | p2p config  |

P2pConfig

| 名称    | 描述      |
| ------- | --------- |
| initStr | initStr   |
| p2pKey  | p2p Id    |
| ices    | ices data |

> The p2pId, pwd, localkey, initStr, and p2pkey parameters are required for TuyaCameraSDK initialization. If these parameters do not correspond to success, the p2p build channel cannot be successful.



### Sample Code 

```java
private void getApi() {
        Map postData = new HashMap();
        postData.put("devId", devId);
        CameraBusiness cameraBusiness = new CameraBusiness();
        cameraBusiness.requestCameraInfo(devId, new Business.ResultListener<CameraInfoBean>() {
            @Override
            public void onFailure(BusinessResponse businessResponse, CameraInfoBean cameraInfoBean, String s) {
                ToastUtil.shortToast(CameraPanelActivity.this, "get cameraInfo failed");
            }

            @Override
            public void onSuccess(BusinessResponse businessResponse, CameraInfoBean cameraInfoBean, String s) {
                configCameraBean = new ConfigCameraBean();

                infoBean = cameraInfoBean;

                mP2p3Id = infoBean.getId();
                p2pType = infoBean.getP2pSpecifiedType();
                p2pId = infoBean.getP2pId().split(",")[0];
                p2pWd = infoBean.getPassword();
                mInitStr = infoBean.getP2pConfig().getInitStr();
                mP2pKey = infoBean.getP2pConfig().getP2pKey();
                mInitStr += ":" + mP2pKey;
                if (null != infoBean.getP2pConfig().getIces()) {
                    token = infoBean.getP2pConfig().getIces().toString();
                }
                configCameraBean.setDevId(devId);
                configCameraBean.setLocalKey(localKey);
                configCameraBean.setInitString(mInitStr);
                configCameraBean.setToken(token);
                configCameraBean.setP2pType(p2pType);
                configCameraBean.setLocalId(mlocalId);
                configCameraBean.setPassword(p2pWd);
                if (P2P_2 == p2pType) {
                    configCameraBean.setP2pId(p2pId);
                }else if (P2P_4 == p2pType){
                    configCameraBean.setP2pId(mP2p3Id);
                }
                initCameraView();
            }
        });
    }	
```

