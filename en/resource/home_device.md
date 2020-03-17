# Home & Equipment



## Family management

After the user login in successfully, he needs to get the information of the entire family list through `TuyaHomeSdk.getHomeManagerInstance ()`, and then initialize one of the families,` ITuyaHomeManager`, to get the details of the family, and manage and control the devices in the family. Refer to the document [Family Management](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/Home_Manager.html).



## Equipment management

Tuya Smart provides rich interfaces for developers to achieve device information acquisition and management capabilities (removal, etc.). The device-related return data is notified to the recipient using an asynchronous message. Refer to the document [Equipment Management](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/Device.html).



**Precautions**

- Device control must first initialize the data, that is, `TuyaHomeSdk.newHomeInstance(homeId).getHomeDetail(ITuyaHomeResultCallback callback)`
- If device control needs to use the latitude and longitude, you need to call `TuyaSdk.setLatAndLong `before re-networking, where lon and lat are used to indicate latitude and longitude information.

