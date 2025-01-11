# LVGL 按钮部件学习

按钮部件 (`lv_btn`) 是 LVGL 中常用的组件之一。和基础对象相比，按钮部件本身没有新增功能，主要是其默认样式不同。

## 按钮部件的组成部分

按钮部件仅包含一个部分：
- **主体 (`lv_part_main`)**：按钮的主要部分。

⚠️ 注意：按钮本身不能显示文本，需要在其上添加 **标签部件 (`lv_label`)** 来显示文字内容。

---

## 学习目标

1. 了解按钮部件的基本创建方法。
2. 学习按钮部件的样式设置。
3. 掌握按钮事件的处理方式。
4. 通过一个完整的电机速度控制面板示例，学以致用。

---

## 1. 按钮部件的基本创建

以下是创建一个按钮的基本步骤：
```c
// 创建按钮并设置大小、位置
lv_obj_t *btn = lv_btn_create(lv_scr_act());
lv_obj_set_size(btn, 100, 50);
lv_obj_align(btn, LV_ALIGN_CENTER, 0, 0);

// 设置按钮的背景颜色
lv_obj_set_style_bg_color(btn, lv_color_hex(0x258EEA), LV_STATE_DEFAULT);
lv_obj_set_style_bg_color(btn, lv_color_hex(0x123456), LV_STATE_PRESSED);

// 给按钮部件添加标志位：可被选中
lv_obj_add_flag(btn_stop, LV_OBJ_FLAG_CHECKABLE); 

// 添加事件回调
lv_obj_add_event_cb(btn, event_cb, LV_EVENT_CLICKED, NULL);
```
- 如果要让按钮切换颜色，需要给部件添加`LV_OBJ_FLAG_CHECKABLE`标志位  
常用标志位：
    | 标志位 | 描述 |
    | ---- | ---- |
    | LV_OBJ_FLAG_CLICKABLE | 使对象可被输入设备点击。 |
    | LV_OBJ_FLAG_CLICK_FOCUSABLE | 点击对象时为其添加焦点状态。 |
    | LV_OBJ_FLAG_CHECKABLE | 点击对象时切换其选中状态。 |
    | LV_OBJ_FLAG_PRESS_LOCK | 按下对象后，即使按压滑出对象区域也保持按下状态。 |
    | LV_OBJ_FLAG_EVENT_BUBBLE | 事件冒泡，将事件传播给父对象。 |
    | LV_OBJ_FLAG_GESTURE_BUBBLE | 手势冒泡，将手势传播给父对象。 |
    | LV_OBJ_FLAG_SNAPPABLE | 如果父对象启用了滚动捕捉功能，则对象可被捕捉。 |
    | LV_OBJ_FLAG_SCROLL_ON_FOCUS | 当对象被聚焦时，自动滚动以使其可见。 |
    | LV_OBJ_FLAG_ADV_HITTEST | 允许执行更精确的点击测试，例如考虑圆角区域。 |

## 2. 按钮部件的事件处理

按钮部件支持多种事件类型，例如：

- LV_EVENT_CLICKED：按钮被点击。
- LV_EVENT_VALUE_CHANGED：按钮值改变（通常用于可选中按钮）。
### 事件回调函数示例

```c
static void event_cb(lv_event_t *e)
{
    lv_event_code_t code = lv_event_get_code(e); // 获取事件类型
    if (code == LV_EVENT_VALUE_CHANGED)
    {
        printf("Button is pressed\n");
    }
}
```

## 3. 电机速度控制面板示例

### 示例简介

此示例实现了一个简单的电机速度控制面板，包含三个按钮和一个显示速度的标签：

1. **Speed+ 按钮**：增加速度。
2. **Speed- 按钮**：减小速度。
3. **Stop 按钮**：停止电机（速度归零）。
4. **速度显示标签**：实时显示当前速度。
### 代码实现

#### 全局变量

```c
lv_obj_t *btn_plus;
lv_obj_t *btn_minus;
lv_obj_t *btn_stop;
lv_obj_t *label_speed;

int speed_val; // 当前速度值
```

#### 按钮事件回调函数

```c
static void event_btn_cb(lv_event_t *e)
{
    lv_obj_t *target = lv_event_get_target(e); // 获取触发源
    lv_event_code_t code = lv_event_get_code(e);

    if (target == btn_plus)
    {
        printf("plus\n");
        speed_val += 30;
    }
    else if (target == btn_minus)
    {
        printf("minus\n");
        speed_val -= 30;
    }
    else if (target == btn_stop)
    {
        printf("stop\n");
        speed_val = 0;
    }

    // 更新速度显示
    lv_label_set_text_fmt(label_speed, "Speed: %d", speed_val);
}
```

#### Speed+ 按钮

```c
btn_plus = lv_btn_create(lv_scr_act());
lv_obj_set_size(btn_plus, 200, 100);
lv_obj_align(btn_plus, LV_ALIGN_LEFT_MID, 50, 0);
lv_obj_set_style_bg_color(btn_plus, lv_color_hex(0x0099FF), LV_STATE_DEFAULT);

lv_obj_t *label_plus = lv_label_create(btn_plus);
lv_label_set_text(label_plus, "Speed+");
lv_obj_set_style_text_font(label_plus, &lv_font_montserrat_24, LV_STATE_DEFAULT);
lv_obj_align(label_plus, LV_ALIGN_CENTER, 0, 0);

lv_obj_add_event_cb(btn_plus, event_btn_cb, LV_EVENT_CLICKED, NULL);
```

#### Speed- 按钮

```c
btn_minus = lv_btn_create(lv_scr_act());
lv_obj_set_size(btn_minus, 200, 100);
lv_obj_align(btn_minus, LV_ALIGN_CENTER, 0, 0);
lv_obj_set_style_bg_color(btn_minus, lv_color_hex(0x0099FF), LV_STATE_DEFAULT);

lv_obj_t *label_minus = lv_label_create(btn_minus);
lv_label_set_text(label_minus, "Speed-");
lv_obj_set_style_text_font(label_minus, &lv_font_montserrat_24, LV_STATE_DEFAULT);
lv_obj_align(label_minus, LV_ALIGN_CENTER, 0, 0);

lv_obj_add_event_cb(btn_minus, event_btn_cb, LV_EVENT_CLICKED, NULL);
```

#### Stop 按钮

```c
btn_stop = lv_btn_create(lv_scr_act());
lv_obj_set_size(btn_stop, 200, 100);
lv_obj_align(btn_stop, LV_ALIGN_RIGHT_MID, -50, 0);
lv_obj_set_style_bg_color(btn_stop, lv_color_hex(0xEF5F60), LV_STATE_DEFAULT); // 默认颜色
lv_obj_set_style_bg_color(btn_stop, lv_color_hex(0x05215F), LV_STATE_PRESSED); // 按下颜色
lv_obj_add_flag(btn_stop, LV_OBJ_FLAG_CHECKABLE); // 可选中

lv_obj_t *label_stop = lv_label_create(btn_stop);
lv_label_set_text(label_stop, "Stop");
lv_obj_set_style_text_font(label_stop, &lv_font_montserrat_24, LV_STATE_DEFAULT);
lv_obj_align(label_stop, LV_ALIGN_CENTER, 0, 0);

lv_obj_add_event_cb(btn_stop, event_btn_cb, LV_EVENT_CLICKED, NULL);
```

#### 速度显示标签

```c
label_speed = lv_label_create(lv_scr_act());
lv_obj_set_style_text_font(label_speed, &lv_font_montserrat_24, LV_STATE_DEFAULT);
lv_obj_align(label_speed, LV_ALIGN_TOP_MID, 0, 50);
lv_label_set_text_fmt(label_speed, "Speed: %d", speed_val);
```

### 示例运行效果

1. 点击 **Speed+ 按钮**，速度值每次增加 30。
2. 点击 **Speed- 按钮**，速度值每次减小 30。
3. 点击 **Stop 按钮**，速度值重置为 0。
4. 当前速度值实时显示在屏幕顶部。
