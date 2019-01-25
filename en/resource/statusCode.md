# Status Code



Status Code：

```c
CONNECTION_STATE_NONE               // 0
CONNECTION_STATE_CONNECTING         // 1 p2p conecting
CONNECTION_STATE_CONNECTED          // 2 p2p connect successful
CONNECTION_STATE_DISCONNECTED       // 3 p2p disconect
CONNECTION_STATE_UNKNOWN_DEVICE     // 4 Unrecognized device 
CONNECTION_STATE_WRONG_PASSWORD     // 5 p2p password error
CONNECTION_STATE_TIMEOUT            // 6 p2p connection timeout
CONNECTION_STATE_UNSUPPORTED        // 7 p2p connection status unsupported
CONNECTION_STATE_CONNECT_FAILED     // 8 p2p connection failed
CONNECTION_STATE_UNKNOWN_LICENSE    // 9 device not licensed

kTuyaErrCode_NoErr                  // 0x00    NoErr
kTuyaErrCode_TimeOut.               // -0x01   TimeOut
kTuyaErrCode_InvalidCommand         // -0x02   Invalid command
kTuyaErrCode_SessionInvalid         // -0x03   Invalid session
kTuyaErrCode_ParamsInvalid          // -0x04   invalid parameters
kTuyaErrCode_ConnectionCancelled    // -0x05   connection cancelled
kTuyaErrCode_DeviceNotOnline        // -0x06   device offline
kTuyaSessionStatus_Connected        // 0       connected
kTuyaSessionStatus_ConnectTimeout   // -3      connect timeout
kTuyaSessionStatus_ClosedRemote     // -12     connection closed by device
kTuyaSessionStatus_ClosedTimeout    // -13     connection closed with no response timeout
kTuyaVideoClarityStandard           // 0x02    SD   
kTuyaVideoClarityHigh               // 0x04    HD
```
