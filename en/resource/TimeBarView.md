# TimeBarView



## Import

Add dependencies of build.gradle under the module:

```groovy
implementation 'com.tuya.smart:tuyasmart-timebar:1.0.1'
```



## Introduction

Time Bar View,  the function is as follows:

- Inherited View
- Suitable for portrait and landscape
- Zoom function (hour, minute, second level)
- Select time in timeline
- Dynamically set time of day
- Show effective video clip time
- There are callbacks such as start of drag, end of drag, automatic movement, etc.
- With valid video clip selection callback
- Free configuration of attributes



## Effect map

<img src="./images/time_bar_view_show.png" alt="Screenshot_1583483189" style="zoom:33%;" />



## Use

### layout

```xml
    <com.tuya.smart.timebarview.TimeBarView
        android:id="@+id/time_bar"
        android:layout_width="match_parent"
        android:layout_height="120dp"/>
```



### Configuration properties (for more property descriptions, see Layout File Property Configuration)

```xml
<com.tuya.smart.timebarview.TimeBarView
        android:id="@+id/time_bar"
        android:layout_width="match_parent"
        android:layout_height="120dp"
        app:bottomTextMargin="4dp"
        app:smallRulerHeight="56dp"
        app:topTextMargin="6dp"/>
```





### Set the current time

```java
public synchronized void setCurrentTimeInMillisecond(long currentTimeInMillisecond)
```

**Parameter Description**

| Parameter                | Description                     |
| ------------------------ | ------------------------------- |
| currentTimeInMillisecond | Hour, minute, and second levels |

**Sample code**

```java
mTimeBarView.setCurrentTimeInMillisecond(System.currentTimeMillis());
```



### Set timeline level

```java
public void setMode(int scalMode)
```

**Parameter Description**

| Parameter | Description                     |
| --------- | ------------------------------- |
| model     | Hour, minute, and second levels |

**Sample code**

```java
mTimeBarView.setMode(2);
```



### Set video time period

```java
public void setRecordDataExistTimeClipsList(List<TimePieceBean> recordDataExistTimeClipsList)
```

**Parameter Description**

| Parameter                    | Description        |
| ---------------------------- | ------------------ |
| recordDataExistTimeClipsList | Time-slice dataset |

**Sample code**

```java
List<TimePieceBean> times = new ArrayList<>();
        TimePieceBean timePieceBean = new TimePieceBean();
        timePieceBean.setStartTime((int) (CalendarUtils.getTodayStart(System.currentTimeMillis())/1000 + 60 * 60));
        timePieceBean.setEndTime((int) (CalendarUtils.getTodayStart(System.currentTimeMillis())/1000 + 120 * 60));
        TimePieceBean timePieceBean2 = new TimePieceBean();
        timePieceBean2.setStartTime((int) (CalendarUtils.getTodayStart(System.currentTimeMillis())/1000 + 120 * 60));
        timePieceBean2.setEndTime((int) (CalendarUtils.getTodayStart(System.currentTimeMillis())/1000 + 180 * 60));
        times.add(timePieceBean);
        times.add(timePieceBean2);
        mTimeBarView.setRecordDataExistTimeClipsList(times);
```



### set Orientation

```java
public void setChangeOrientation(int orientation)
```

**Parameter Description**

| Parameter   | Description                    |
| ----------- | ------------------------------ |
| orientation | Android view orientation value |

**Sample code**

```java
    @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        mTimeBarView.setChangeOrientation(newConfig.orientation);
    }
```



### Layout file property configuration

> All properties have default values and can be left unset

**Parameter Description**

| Attribute        | Description        |
| ---------------- | ------------------ |
| bottomTextMargin | bottom text margin |
| smallRulerHeight | small rule height  |
| topTextMargin    | top text margin    |



## Obfuscation configuration



```java
#timebarview
-keep class com.tuya.smart.**{*;}
-dontwarn com.tuya.smart.**
```

