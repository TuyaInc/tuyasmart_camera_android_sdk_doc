# CameraBusiness

获取IPC Config数据的实现类

## 获取IPC Config数据方法

```java
void requestCameraInfo(String devId, ResultListener<CameraInfoBean> listener)
```

### 参数说明

| 参数              |      描述      |
| -------------------- | ------------ | --------------- |
| devId  | 设备Id |
| listener  | 数据回调 |

CameraInfoBean

| 名称                     | 描述   |
| ------------------------ | ------ |
| password                   | 秘钥   |
| p2pId               | p2p Id |
| p2pSpecifiedType         | p2p的类型   |
| P2pConfig             | p2p配置信息 |

P2pConfig

| 名称                     | 描述   |
| ------------------------ | ------ |
| initStr                   | initStr   |
| p2pKey               | p2p Id |
| ices         | ices数据   |

> p2pId，pwd, localkey,initStr,p2pkey这几个参数是TuyaCameraSDK初始化所必须有的，这几个参数没有对应成功，p2p的构建通道就不可能成功。



### 示例代码：

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
