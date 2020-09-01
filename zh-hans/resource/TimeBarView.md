# TimeBarView



## 引用

模块下的 build.gradle 的依赖添加：

```groovy
implementation 'com.tuya.smart:tuyasmart-timebar:1.0.1'
```



## 描述

时间刻度尺，功能如下：

- 继承 View
- 适配横竖屏
- 缩放功能(小时、分钟、秒级别)
- 时间轴中选择时间
- 动态设置当天时间
- 显示有效的视频片段时间
- 有拖动开始、拖动结束、自动移动等回调
- 带有效视频片段选择回调
- 属性自由配置



## 效果图

<img src="./images/time_bar_view_show.png" alt="Screenshot_1583483189" style="zoom:33%;" />

## 使用

### 布局

```xml
    <com.tuya.smart.timebarview.TimeBarView
        android:id="@+id/time_bar"
        android:layout_width="match_parent"
        android:layout_height="120dp"/>
```

#### 配置属性（更多属性说明见布局文件属性配置）

```xml
    <com.tuya.smart.timebarview.TimeBarView
        android:id="@+id/time_bar"
        android:layout_width="match_parent"
        android:layout_height="120dp"
        app:bottomTextMargin="4dp"
        app:smallRulerHeight="56dp"
        app:topTextMargin="6dp"/>
```



### 设置当前时间

```java
public synchronized void setCurrentTimeInMillisecond(long currentTimeInMillisecond)
```

**参数说明**

| 参数                     | 描述           |
| :------------------------ | :-------------- |
| currentTimeInMillisecond | 时、分、秒级别 |

** 示例代码**

```java
mTimeBarView.setCurrentTimeInMillisecond(System.currentTimeMillis());
```

### 设置时间轴级别

```java
public void setMode(int scalMode)
```

**参数说明**

| 参数  | 描述                    |
| :----- | :----------------------- |
| model | 分为时、分、秒 三个级别 |

**示例代码**

```java
mTimeBarView.setMode(2);
```



### 设置视频时间段

```java
public void setRecordDataExistTimeClipsList(List<TimePieceBean> recordDataExistTimeClipsList)
```

**参数说明**

| 参数                         | 描述           |
| :---------------------------- | :-------------- |
| recordDataExistTimeClipsList | 时间片段数据集 |



**示例代码**

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



### 横竖屏操作

```java
public void setChangeOrientation(int orientation)
```

**参数说明**

| 参数        | 说明          |
| :----------- | :------------------ |
| orientation | 横屏/竖屏的 value 值 |

**示例代码**

```java
    @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        mTimeBarView.setChangeOrientation(newConfig.orientation);
    }
```



### 布局文件属性配置

> 所有属性都有默认值，可以不设置

| 属性             | 描述                |
| :---------------- | :------------------- |
| bottomTextMargin | 底部刻度值的 margin |
| smallRulerHeight | 短刻度线的高度      |
| topTextMargin    | 顶部刻度值的 margin |



## 混淆配置

```java
#timebarview
-keep class com.tuya.smart.**{*;}
-dontwarn com.tuya.smart.**
```







