#  程序架构说明



## SDK架构

### 介绍
IPC Camera SDK 组成分为Viewer模块、中间件模块、消息中心模块、设备信令模块、IOTCamera模块和辅助功能模块。

- Viewer模块主要是视频播放器显示控件。
- 中间件模块提供隔离业务和SDK的接口功能。
- 消息仲系模块提供业务所需的IPC设备告警消息功能。
- 设备信令模块提供基于Mqtt的设备控制信令数据收发功能。
- IOTCamera模块提供基于IP Camera P2P功能及音视频数据收发功能。
- 辅助功能模块提供一系列的辅助功能

### 架构图展示

![image](./images/ProgramArchitecure.png)


### 主要组件模块：

IPC Camera SDK包含4个模块：

 - tuyasmart-ipc-camera-middleware：Tuya IPC SDK与业务层关联的中间件
 - tuyasmart-ipc-camera-v1： 提供了Tuya IPC的P2P 1.0的功能 (废弃)
 - tuyasmart-ipc-camera-v2： 提供了Tuya IPC的P2P功能
 - tuyasmart-ipc-camera-utils：提供了一些辅助类和函数
 - tuyasmart-ipc-devicecontrol：提供下发与接收设备控制指令
