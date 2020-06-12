# Device Configuration

The network configuration modes supported by Tuya smart camera hardware module including:

* Quick connection mode（TLink, it is referred to as the EZ mode）
* Hotspot mode (AP mode)
* Qr code mode

> Qr code mode is relatively simple, it is recommended to use the qr code mode, if the device can not scan the qr code, then try quick connection mode.

Due to Mini SDK and Full SDK, the device activation process and code will be slightly different.

**EZ mode** and **AP mode** is same as other Tuya device，Mini SDK solution refer to [TuyaSmartActivator Android SDK](https://github.com/TuyaInc/tuyasmart_android_activator_sdk), Full SDK solution refer to [ Network Configuration](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/Activator_device.html).

The following focuses on the smart camera's unique Qr code mode.


### Binding mode

Tuya smart camera support three binding modes: strong, medium and weak. After the device is successfully activated into the home of the corresponding account, different binding modes and different verification methods are required for unbinding.

* **Strong mode**：Only after the previous user removes the device from the App, the device can be reconfigured and bound to another account.
* **Medium mode**：Without the previous user removing the device from the App, the device can be reconfigured and bound to another account, but a PUSH notification will be sent to the previous account.
* **Weak mode**：Without the previous user removing the device from the App, you can reconfigure the device to bind to another account.

Tuya smart camera just support strong binding mode, and can't change to other mode. If you has a strong demand and has evaluated the impact of modifying to other modes, you can submit an order to the Tuya developer platform.

