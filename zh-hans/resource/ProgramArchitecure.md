#  程序架构说明



## SDK架构



### 简介

**Tuya IPC Android SDK**基于**涂鸦全屋智能SDK**封装了智能摄像机的相关功能。整个SDK架构组成分为三个层级：涂鸦全屋智能SDK、IPC网络通信层、摄像机业务层。

- 涂鸦全屋智能SDK提供了以家庭为单位，对硬件设备、涂鸦云通讯等接口封装。
- IPC网络通信层提供了P2P网络通道实现。
- 摄像机业务层提供了音视频通讯和展示、设备功能点、报警消息、云存储视频管理等业务功能。

> note: IPC 是IP Camera的简称，中文称为网络摄像机,由网络编码模块和[模拟摄像机](https://baike.baidu.com/item/模拟摄像机/4419990)组合而成。



### 架构图

![image](./images/ProgramArchitecure.jpg)



### 组件模块

模块组成分为视图模块、中间件模块、消息中心模块、设备信令模块、P2P通信模块和基础工具库模块。

- 视图模块主要是摄像机相关UI组件，如：视频播放器显示控件。
- 中间件模块提供隔离业务和SDK的接口功能。
- 消息中心模块提供业务所需的IPC设备告警消息功能。
- 设备信令模块提供基于Mqtt的设备控制信令数据收发功能。
- IOTCamera模块提供基于IP Camera P2P功能及音视频数据收发功能。
- 辅助功能模块提供一系列的辅助功能



#### 模块说明

| 模块                            | 说明                                 |
| ------------------------------- | ------------------------------------ |
| tuyasmart-ipc-camera-middleware | Tuya IPC SDK与业务层关联的中间件     |
| tuyasmart-ipc-camera-v1         | 提供了Tuya IPC的P2P 1.0的功能 (废弃) |
| tuyasmart-ipc-camera-v2         | 提供了Tuya IPC的P2P网络通信功能      |
| tuyasmart-ipc-devicecontrol     | 提供下发与接收设备控制指令           |
| tuyasmart-ipc-camera-utils      | 提供了一些辅助类和函数               |