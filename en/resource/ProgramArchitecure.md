#  program architecture statement



## SDK Architecture



### Introduction

**Tuya IPC Android SDK** is based on the **Tuya  Smart  SDK** and encapsulates the functions of smart cameras. The entire SDK architecture is divided into three levels: Tuya Smart SDK, IPC network communication layer, and camera business layer.

	- **Tuya  Smart  SDK**  provides a home-based package for hardware devices and Tuya cloud communication.
	- IPC network communication layer provides P2P network channel implementation.
	- The camera business layer provides audio and video communication and display, equipment function points, alarm messages, cloud storage video management and other business functions.



### Architecture diagram

![image](./images/ProgramArchitecure.jpg)



### Component module

The module consists of a view module, a middleware module, a message center module, a device signaling module, a P2P communication module, and a basic tool library module.

- The view module is mainly camera-related UI components, such as video player display controls.
- The middleware module provides interface functions that isolate services from the SDK.
- The message center module provides the alarm message function of the IPC equipment required by the service.
- The device signaling module provides Mqtt-based device control signaling data transmission and reception functions.
- IOTCamera module provides IP Camera P2P function and audio and video data transmission and reception functions.
- Auxiliary function module provides a series of auxiliary functions.



### Module description

| Module                          | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| tuyasmart-ipc-camera-middleware | Tuya IPC SDK middleware associated with business layer       |
| tuyasmart-ipc-camera-message    | the alarm message function of the IPC equipment required by the service |
| tuyasmart-ipc-camera-v2         | provides IP Camera P2P function and audio and video data transmission and reception functions. |
| tuyasmart-ipc-devicecontrol     | provides Mqtt-based device control signaling data transmission and reception functions. |
| tuyasmart-ipc-camera-utils      | provides a series of auxiliary functions.                    |