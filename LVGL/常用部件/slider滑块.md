# LVGL 滑块组件（lv_slider）学习笔记

`lv_slider` 是 LVGL 中用于调节参数值的组件，通过直线滑动修改数值，适合实现范围调节、对称滑动或单值滑动的功能。

## 滑块组件的组成部分

- **主体lv_part_main**滑块的背景部分，决定滑块整体外观。
- **指示器lv_part_indicator**用于标记滑块当前的数值范围。
- **把手lv_part_knob**滑块的可拖动部分，用于用户调整滑块值。
## 滑块组件的创建与基本设置

### 1. **创建滑块组件**

```c
lv_obj_t *slider = lv_slider_create(lv_scr_act());
lv_obj_set_size(slider, 200, 20);  // 设置滑块的宽高
lv_obj_align(slider, LV_ALIGN_CENTER, 0, 0);  // 对齐到屏幕中心
```

**说明**：

- 滑块通过 lv_slider_create 创建，父对象设置为当前活动屏幕（lv_scr_act()）。
- 使用 lv_obj_set_size 设置滑块的宽度和高度。
### 2. **设置滑块的数值范围**

```c
lv_slider_set_range(slider, 0, 100);
```

**说明**：

- 使用 lv_slider_set_range 设置滑块的取值范围，此处范围为 0 到 100。
### 3. **设置滑块的初始值**

```c
lv_slider_set_value(slider, 50, LV_ANIM_OFF);
```

**说明**：

- 调用 lv_slider_set_value 设置滑块当前的值。
- 第三个参数控制是否显示动画，LV_ANIM_OFF 表示关闭动画，LV_ANIM_ON 表示启用动画。
## 滑块的交互与回调

### 1. **获取滑块的当前值**

```c
int value = lv_slider_get_value(slider);
```

**说明**：

- 使用 lv_slider_get_value 获取滑块当前的值，返回值为整型。
### 2. **设置滑块的事件回调函数**

```c
static void event_slider_cb(lv_event_t* e)
{
    lv_obj_t *slider = lv_event_get_target(e);
    printf("当前值是：%d\n", lv_slider_get_value(slider));
}

lv_obj_add_event_cb(slider, event_slider_cb, LV_EVENT_VALUE_CHANGED, NULL);
```

**说明**：

- 通过 lv_obj_add_event_cb 添加回调函数，监听滑块的数值变化事件（LV_EVENT_VALUE_CHANGED）。
- 回调函数中可以获取滑块的当前值，并进行相关操作。
## 滑块的模式设置

### 1. **滑块模式介绍**

`lv_slider` 支持三种模式：

- **LV_SLIDER_MODE_NORMAL（正常模式）**滑块在最小值和最大值之间移动，仅支持单值调节。
- **LV_SLIDER_MODE_SYMMETRICAL（对称模式）**滑块围绕中间点对称地移动，适用于正负值调节。
- **LV_SLIDER_MODE_RANGE（范围模式）**滑块支持范围选择，用户可以调节两个值。
### 2. **设置滑块模式**

```c
lv_slider_set_mode(slider, LV_SLIDER_MODE_RANGE);
```

**说明**：

- 通过 lv_slider_set_mode 设置滑块的工作模式，此处设置为范围模式（LV_SLIDER_MODE_RANGE）。
## 范围模式下的额外功能

### 1. **设置与获取左值**

在范围模式下，滑块有两个值：

- **右值**：由用户拖动把手调节。
- **左值**：可通过代码设置，表示范围的起点。
#### 设置左值

```c
lv_slider_set_left_value(slider, 20, LV_ANIM_OFF);
```

#### 获取左值

```c
int left_value = lv_slider_get_left_value(slider);
```

**说明**：

- 左值不能大于右值，否则设置会无效。