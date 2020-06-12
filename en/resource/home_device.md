# Home and device



## Home management

After the user login in successfully, he needs to get the information of the entire family list through `TuyaHomeSdk.getHomeManagerInstance()`, and then initialize one of the families,` ITuyaHomeManager`, to get the details of the family, and manage and control the devices in the family. Refer to the document [Family Management](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/HomeManager.html).



## Device management

Tuya Smart provides rich interfaces for developers to achieve device information acquisition and management capabilities (removal, etc.). The device-related return data is notified to the recipient using an asynchronous message. Refer to the document [Equipment Management](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/Device.html).


**Precautions**

- Device control must first initialize the data, that is, `TuyaHomeSdk.newHomeInstance(homeId).getHomeDetail(ITuyaHomeResultCallback callback)`
- If device control needs to use the latitude and longitude, you need to call `TuyaSdk.setLatAndLong `before re-networking, where lon and lat are used to indicate latitude and longitude information.

## Camera device

After obtaining the device list, you can determine whether it is a smart camera device according to the type of device. If it is a smart camera device, you can create a camera object based on the information in DeviceBean.

### Determine if it is a smart camera

The type of device can be judged according to the category property of DeviceBean, and the type of smart camera is `sp`.

**Example**

```java
private void gotoDeviceCommonActivity(DeviceBean devBean) {
    if ("sp".equals(devBean.getProductBean().getCategory())) {
        //is smart camera
    }
}
```

### Camera configuration information

After filtering out the smart camera devices, you can obtain the camera configuration information based on the device id, which is the `devId` attribute of DeviceBean. See [Get Device Information](./LiveProcess.html) for details.

### P2p type

The Tuya smart camera supports three p2p channel implementation solutions. The Camera SDK will initialize different camera objects according to the p2p type. Obtain the p2p type of the device as follows:

```java
Map<String, Object> map = deviceBean.getSkills();
int p2pType = -1;
if (map == null || map.size() == 0) {
    p2pType = -1;
} else {
    p2pType = (Integer) (map.get("p2pType"));
}
```

