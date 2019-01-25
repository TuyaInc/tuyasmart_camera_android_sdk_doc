# 状态码



状态码：

```c
CONNECTION_STATE_NONE               // 0
CONNECTION_STATE_CONNECTING         // 1 p2p正在连接
CONNECTION_STATE_CONNECTED          // 2 p2p连接成功
CONNECTION_STATE_DISCONNECTED       // 3 p2p已断开
CONNECTION_STATE_UNKNOWN_DEVICE     // 4 未识别的设备
CONNECTION_STATE_WRONG_PASSWORD     // 5 p2p密码错误
CONNECTION_STATE_TIMEOUT            // 6 p2p连接超时
CONNECTION_STATE_UNSUPPORTED        // 7 p2p连接状态不支持
CONNECTION_STATE_CONNECT_FAILED     // 8 p2p连接失败
CONNECTION_STATE_UNKNOWN_LICENSE    // 9 设备未许可

kTuyaErrCode_NoErr                  // 0x00   无错
kTuyaErrCode_TimeOut.               // -0x01  超时
kTuyaErrCode_InvalidCommand         // -0x02  无效的命令
kTuyaErrCode_SessionInvalid         // -0x03  无效的session
kTuyaErrCode_ParamsInvalid          // -0x04  无效的参数
kTuyaErrCode_ConnectionCancelled    // -0x05  连接被取消
kTuyaErrCode_DeviceNotOnline        // -0x06  设备不在线
kTuyaSessionStatus_Connected        // 0      链接成功
kTuyaSessionStatus_ConnectTimeout   // -3     链接超时
kTuyaSessionStatus_ClosedRemote     // -12    链接被设备关闭
kTuyaSessionStatus_ClosedTimeout    // -13    链接无响应超时关闭
kTuyaVideoClarityStandard           // 0x02   标清    
kTuyaVideoClarityHigh               // 0x04   高清
```