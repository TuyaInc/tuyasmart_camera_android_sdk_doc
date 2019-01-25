# Camera Device Data Point



data point definite the status of current device. about the data point please check[Device Control](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/Device.html)。

check the definition of data point in [tuya iot platform]((https://iot.tuya.com/login)) , if the data point of controlling the ptz is' 119', the data point of stop controlling ptz is '116', definite “0”、“4”、“6”、“2”  as up, down,left,right at device part , the code for controling the ptz is below


```java

    @Override
    public boolean isSupportPTZ() {  // whether support PTZ or not
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
        mTuyaDevice.publishDps(dps, new IResultCallback() {
            @Override
            public void onError(String s, String s1) {
                if (mOnOperatorMsgListener != null && bean != null) {
                    mOnOperatorMsgListener.onClearMsg(bean.getId());
                }

                Log.e(TAG, "dpOperate error " + s + " msg " + s1);
                mDpCamOperatorManager.notifyFailByID(dpClassId, s, s1);
            }

            @Override
            public void onSuccess() {
                //do nothing,wait for dp report
            }
        });
    }
```

