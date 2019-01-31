# 摄像头设备功能点



设备功能点定义了当前设备的状态，称作数据点（DP点），详细的定义与介绍查看[设备控制](https://docs.tuya.com/cn/app/app-sdk/android-sdk.html#6%E8%AE%BE%E5%A4%87%E6%8E%A7%E5%88%B6)。

在[涂鸦开发者平台](https://iot.tuya.com/login)查询产品功能点的定义，如假设开始云台控制的功能点 id 为 “119”，停止云台控制的功能点 id 为 “116”，设备端以 “0”、“4”、“6”、“2” 标记上下左右四个方向，那么云台控制的示例代码如下：

```java

    @Override
    public boolean isSupportPTZ() {  //是否支持云台控制
        return mDpCamOperatorManager.querySupportByID(DpPTZ.ID);
    }


    @Override
    public void startPtz(PTZDirection direction) {
        dpOperate(DpPTZ.ID, direction, null);
    }

	@Override
    public void stopPtz() {
        dpOperate(DpPTZStop.ID, true, null);
    }

    private void dpOperate(final String dpClassId, Object value, final OperatorMsgBean bean) {
        String dps = mDpCamOperatorManager.generateDps(dpClassId, value);
        if (bean != null) {
            bean.setId(dpClassId);
            bean.setCommandCode(dps);
        }
        Log.d(TAG, "dpOperate " + dps);
        mDevice.send(dps, callback);
    }
```

### 摄像头基本功能
| DpId | value | 描述 |
| ------ | ------ | ------ |
| "101" | {"101":false} | 状态指示灯 |
| "104" | {"104":false} | 时间水印|
| "103" | {"103":true} | 画面翻转|
| "108" | {"108":"2"} | 红外夜视功能|