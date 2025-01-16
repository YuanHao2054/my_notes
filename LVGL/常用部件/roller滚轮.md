# LVGL 滚轮组件（lv_roller）学习笔记

`lv_roller` 是 LVGL 中用于多选一场景的组件，通过滚轮的形式展现多个选项。

## 滚轮组件的组成部分

- **主体lv_part_main** 滚轮的主要外观部分，用于设置滚轮整体的样式。
- **选项框lv_part_selected** 显示当前选中项的区域，可通过样式自定义选中项的视觉效果。
## 滚轮组件的创建与基本设置

### 1. **创建滚轮组件**

```c
roller = lv_roller_create(lv_scr_act());
lv_obj_set_size(roller, 100, 100);  // 设置滚轮的尺寸
lv_obj_align(roller, LV_ALIGN_CENTER, 0, 0);  // 对齐到屏幕中心
```

**说明**：

- 滚轮使用 lv_roller_create 创建，其父对象设置为当前活动屏幕（lv_scr_act()）。
- 可以通过 lv_obj_set_size 调整滚轮的宽高。
### 2. **设置选项间隔**

```c
lv_obj_set_style_text_line_space(roller, 5, LV_STATE_DEFAULT);
```


### 3. **设置滚轮选项**

```c
lv_roller_set_options(roller, 
    "Apple\nBanana\nOrange\nMelon\nGrape\nRaspberry\nCherry\nPear\nKiwi", 
    LV_ROLLER_MODE_NORMAL);
```

**说明**：

- lv_roller_set_options 用于设置滚轮的选项内容，通过换行符（\n）分隔每个选项。
- 滚轮支持两种滚动模式：**LV_ROLLER_MODE_NORMAL**：正常滚动模式，滚轮到达首尾时停止。**LV_ROLLER_MODE_INFINITE**：无限滚动模式，滚轮首尾相接。
### 4. **设置当前选项**

```c
lv_roller_set_selected(roller, 2, LV_ANIM_OFF);
```

**说明**：

- 使用 lv_roller_set_selected 设置当前选中的选项，索引从 0 开始。
- 第三个参数控制是否显示动画：LV_ANIM_ON 开启动画，LV_ANIM_OFF 关闭动画。
### 5. **设置可见行数**

```c
lv_roller_set_visible_row_count(roller, 6);
```

**说明**：

- 通过 lv_roller_set_visible_row_count 设置滚轮可见的选项行数，此处设置为 6 行。
## 滚轮的交互与回调函数

### 1. **获取选中内容**

#### 获取选项索引

```c
int index = lv_roller_get_selected(roller);
```

#### 获取选项字符串

```c
char buf[32];
lv_roller_get_selected_str(roller, buf, sizeof(buf));
printf("选中的选项是：%s\n", buf);
```

**说明**：

- lv_roller_get_selected 返回当前选中项的索引。
- lv_roller_get_selected_str 获取当前选中项的内容并存储到缓冲区。
### 2. **添加事件回调函数**

```c
static void event_roller_cb(lv_event_t* e)
{
    char buf[32];
    lv_roller_get_selected_str(roller, buf, sizeof(buf));
    printf("选中的选项是：%s\n", buf);
    printf("选中的选项是：%d\n", lv_roller_get_selected(roller));
}

lv_obj_add_event_cb(roller, event_roller_cb, LV_EVENT_VALUE_CHANGED, NULL);
```

**说明**：

- 使用 lv_obj_add_event_cb 为滚轮组件添加事件回调函数。
- 回调函数中通过 LV_EVENT_VALUE_CHANGED 响应选项变化事件，并输出当前选中项的内容和索引。