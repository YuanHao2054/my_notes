# LVGL 进度条部件（lv_bar）学习笔记

## 1. 进度条部件概述

`lv_bar` 是 LVGL 中用于显示当前任务进度的部件。进度条可以显示为水平或垂直的形式，通常用于展示任务的完成度。

### 组成部分

进度条部件由两部分组成：

- **主体（lv_part_main）**：进度条的背景，展示整个进度条的框架。
- **指示器（lv_part_indicator）**：进度条的进度，动态变化的部分，表示当前进度。
## 2. 创建进度条部件

进度条部件的创建分为几个步骤，从创建对象到设置外观、动画等。

### 2.1 创建进度条对象

使用 `lv_bar_create` 函数来创建一个进度条对象。通常，我们将其添加到当前屏幕 (`lv_scr_act()`)。

```c
lv_obj_t *bar1 = lv_bar_create(lv_scr_act());
```

### 2.2 设置进度条大小

通过 `lv_obj_set_size` 设置进度条的尺寸，宽度决定进度条的水平尺寸，长度决定垂直尺寸。

```c
lv_obj_set_size(bar1, 200, 20);  // 横向大于纵向，就是横向进度条，反之就是纵向进度条
```

### 2.3 设置进度条对齐方式

通过 `lv_obj_align` 设置进度条的对齐方式。通常将其置于屏幕中心：

```c
lv_obj_align(bar1, LV_ALIGN_CENTER, 0, 0); // 对齐到屏幕中央
```

## 3. 设置进度条的值与范围

### 3.1 设置进度条的范围

使用 `lv_bar_set_range` 来设置进度条的最小值和最大值，范围限制进度条可以展示的值域。

```c
lv_bar_set_range(bar1, 0, 100);  // 设置进度条的范围从 0 到 100
```

### 3.2 设置进度条的当前值

`lv_bar_set_value` 用于设置当前的进度条值。第三个参数控制是否启用动画效果：

- LV_ANIM_ON：启用动画。
- LV_ANIM_OFF：禁用动画。
```c
// 设置进度条的值，并启用动画效果
lv_bar_set_value(bar1, 70, LV_ANIM_ON);  // 当前值为70
```

### 3.3 设置动画时间

`lv_obj_set_style_anim_time` 用于设置进度条动画的持续时间。动画时间必须在设置当前值之前设置。

```c
lv_obj_set_style_anim_time(bar1, 1000, LV_STATE_DEFAULT);  // 设置动画时间为 1000ms
```

## 4. 设置进度条的模式

进度条有不同的模式，通过 `lv_bar_set_mode` 进行选择。支持以下三种模式：

- **LV_BAR_MODE_NORMAL**：默认模式，表示从设定的范围最小值绘制到当前值。
- **LV_BAR_MODE_SYMMETRICAL**：从零绘制到指定值，指定值可以小于零。
- **LV_BAR_MODE_RANGE**：允许设定起始值，起始值必须小于当前值。
```c
lv_bar_set_mode(bar1, LV_BAR_MODE_RANGE);  // 设置进度条的模式为范围模式
```

## 5. 设置起始值

`lv_bar_set_start_value` 用于设置进度条的起始值。仅在使用 `LV_BAR_MODE_RANGE` 模式时有效。

```c
lv_bar_set_start_value(bar1, 20, LV_ANIM_ON);  // 设置起始值为20，启用动画
```

## 6. 动态更新进度条

### 6.1 使用定时器动态改变进度条的值

可以通过 `lv_timer_create` 创建定时器，定时器回调函数内改变进度条的值。定时器的回调函数可以定期调用 `lv_bar_set_value` 来更新进度条的显示。

```c
// 定时器回调函数，定期更新进度条的值
lv_timer_create(update_bar_value, 1000, bar1);  // 每 1000ms 调用一次 update_bar_value
```

在回调函数中，调用 `lv_bar_set_value` 来更新进度条的值。

```c
void update_bar_value(lv_timer_t * timer)
{
    static int value = 0;
    if (value < 100)
        value += 10;
    else
        value = 0;

    lv_bar_set_value(timer->user_data, value, LV_ANIM_ON);  // 动态更新进度条的值
}
```
