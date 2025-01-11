# LVGL 开关部件（lv_switch）

### 开关部件组成部分：
1. **主体**：`lv_part_main`
2. **滑块**：`lv_part_knob`
3. **指示器**：`lv_part_indicator`

### 1. 事件回调函数

事件回调函数 `event_smart_control_cb` 用于处理开关部件的状态变更事件。

```c
static void event_smart_control_cb(lv_event_t *e)
{
    bool state_cool;
    bool state_heat;
    bool state_dry;

    lv_obj_t *target = lv_event_get_target(e);  // 获取触发源

    if (target == sw_cool) // cool和heat互斥
    {
        state_cool = lv_obj_has_state(sw_cool, LV_STATE_CHECKED); // 获取开关的状态  判断当前状态是否为已打开 返回值为bool类型
        state_heat = lv_obj_has_state(sw_heat, LV_STATE_CHECKED);
        printf("cool state: %d\n", state_cool);
        if (state_cool == 1)
        {
            lv_obj_clear_state(sw_heat, LV_STATE_CHECKED); // 设置为关闭状态 清除状态
        }
    }
    else if (target == sw_heat)
    {
        state_heat = lv_obj_has_state(sw_heat, LV_STATE_CHECKED); // 获取开关的状态  判断当前状态是否为已打开 返回值为bool类型
        state_cool = lv_obj_has_state(sw_cool, LV_STATE_CHECKED);
        printf("heat state: %d\n", state_heat);
        if (state_heat == 1)
        {
            lv_obj_clear_state(sw_cool, LV_STATE_CHECKED); // 设置为关闭状态 清除状态
        }
    }
    else if (target == sw_dry)
    {
        state_dry = lv_obj_has_state(sw_dry, LV_STATE_CHECKED); // 获取开关的状态  判断当前状态是否为已打开 返回值为bool类型
        printf("dry state: %d\n", state_dry);
    }
}
```

### 2. 开关部件的创建与配置

以下是创建一个开关部件并设置其外观和行为的代码片段。

```c
// 创建一个开关部件
sw = lv_switch_create(lv_scr_act());  // 名称不能直接用switch，因为是关键字
lv_obj_align(sw, LV_ALIGN_CENTER, 0, 0);
lv_obj_set_size(sw, 100, 50);
lv_obj_set_style_bg_color(sw, lv_color_hex(0xFBF200), LV_PART_KNOB); // 设置手柄为黄色 默认状态 只放部件时，LV_STATE_DEFAULT | LV_PART_KNOB
lv_obj_set_style_bg_color(sw, lv_color_hex(0x020CFF), LV_STATE_CHECKED | LV_PART_INDICATOR); // 设置指示器为深蓝色 开启状态
lv_obj_set_style_bg_color(sw, lv_color_hex(0x00DF01), LV_PART_MAIN); // 设置主体为绿色 默认状态 当开关状态为开时，指示器会覆盖掉主体的颜色
```

### 3. 修改开关状态

可以通过代码来修改开关部件的状态，而不仅仅依赖于用户触摸操作。

```c
// 添加、清除开关的状态
lv_obj_add_state(sw, LV_STATE_CHECKED); // 设置为打开状态
lv_obj_clear_state(sw, LV_STATE_CHECKED); // 设置为关闭状态 清除状态

// 设置为禁用状态，且保持为已打开
lv_obj_add_state(sw, LV_STATE_DISABLED | LV_STATE_CHECKED); // 设置为禁用状态 打开，且不可修改
lv_obj_remove_state(sw, LV_STATE_DISABLED | LV_STATE_CHECKED); // 移除禁用状态
```

### 4. 添加事件回调

为开关部件添加事件回调，以响应状态变化等事件。

```c
lv_obj_add_event_cb(sw, event_cb, LV_EVENT_VALUE_CHANGED, NULL); // 添加事件
```

### 5. 获取和判断开关状态

可以使用 `lv_obj_has_state` 来获取开关的当前状态并进行判断。

```c
// 获取、判断开关的状态
bool state = lv_obj_has_state(sw, LV_STATE_CHECKED); // 获取开关的状态  判断当前状态是否为已打开 返回值为bool类型
```

# LVGL 智能家居控制系统开关部件示例

## 介绍

本例展示了如何使用 LVGL（Light and Versatile Graphics Library）创建一个智能家居控制系统界面，其中包含三个开关部件（Cool、Heat、Dry）。每个开关控制一个功能，用户可以通过这些开关启用或禁用相应功能。代码包括了开关部件的创建、事件回调的绑定以及布局的实现。

## 代码分析

### 1. 全局变量

首先，定义了三个开关部件指针：

```c
lv_obj_t *sw_cool;
lv_obj_t *sw_heat;
lv_obj_t *sw_dry;
```

这四个对象分别对应一个主开关和三个具体功能开关（冷却、加热和干燥）。

### 2. 事件回调函数

`event_smart_control_cb` 是用于处理开关部件状态变更的回调函数。当开关状态发生变化时，触发此函数，更新相应的开关状态并确保互斥关系（冷却和加热功能互斥）。

```c
static void event_smart_control_cb(lv_event_t *e)
{
    bool state_cool;
    bool state_heat;
    bool state_dry;

    lv_obj_t *target = lv_event_get_target(e);  // 获取触发源

    // 控制冷却和加热开关的互斥
    if (target == sw_cool) 
    {
        state_cool = lv_obj_has_state(sw_cool, LV_STATE_CHECKED); 
        state_heat = lv_obj_has_state(sw_heat, LV_STATE_CHECKED);
        printf("cool state: %d\n", state_cool);
        if (state_cool == 1)
        {
            lv_obj_clear_state(sw_heat, LV_STATE_CHECKED); // 关闭加热
        }
    }
    else if (target == sw_heat)
    {
        state_heat = lv_obj_has_state(sw_heat, LV_STATE_CHECKED);
        state_cool = lv_obj_has_state(sw_cool, LV_STATE_CHECKED);
        printf("heat state: %d\n", state_heat);
        if (state_heat == 1)
        {
            lv_obj_clear_state(sw_cool, LV_STATE_CHECKED); // 关闭冷却
        }
    }
    else if (target == sw_dry)
    {
        state_dry = lv_obj_has_state(sw_dry, LV_STATE_CHECKED); // 获取干燥开关状态
        printf("dry state: %d\n", state_dry);
    }
}
```

### 3. 创建开关部件

接下来，分别为三个功能（冷却、加热、干燥）创建独立的开关部件，并为每个开关添加事件回调。

#### 3.1 创建冷却开关

```c
static void obj_create_sw_cool()
{
    lv_obj_t *sw_cool_parent = lv_obj_create(lv_scr_act());
    lv_obj_set_size(sw_cool_parent, 160, 160);
    lv_obj_align(sw_cool_parent, LV_ALIGN_CENTER, -230, 0);
    lv_obj_set_style_border_opa(sw_cool_parent, 70, 0); // 设置边框透明度

    // 创建标签
    lv_obj_t *label_cool = lv_label_create(sw_cool_parent);
    lv_label_set_text(label_cool, "Cool");
    lv_obj_align(label_cool, LV_ALIGN_CENTER, 0, -30);
    lv_obj_set_style_text_font(label_cool, &lv_font_montserrat_24, LV_STATE_DEFAULT);

    // 创建开关
    sw_cool = lv_switch_create(sw_cool_parent);
    lv_obj_align(sw_cool, LV_ALIGN_CENTER, 0, 20);
    lv_obj_set_size(sw_cool, 80, 40);

    // 添加事件
    lv_obj_add_event_cb(sw_cool, event_smart_control_cb, LV_EVENT_VALUE_CHANGED, NULL);
}
```

#### 3.2 创建加热开关

```c
static void obj_create_sw_heat()
{
    lv_obj_t *sw_heat_parent = lv_obj_create(lv_scr_act());
    lv_obj_set_size(sw_heat_parent, 160, 160);
    lv_obj_align(sw_heat_parent, LV_ALIGN_CENTER, 0, 0);
    lv_obj_set_style_border_opa(sw_heat_parent, 70, 0); 

    // 创建标签
    lv_obj_t *label_heat = lv_label_create(sw_heat_parent);
    lv_label_set_text(label_heat, "Heat");
    lv_obj_align(label_heat, LV_ALIGN_CENTER, 0, -30);
    lv_obj_set_style_text_font(label_heat, &lv_font_montserrat_24, LV_STATE_DEFAULT);

    // 创建开关
    sw_heat = lv_switch_create(sw_heat_parent);
    lv_obj_align(sw_heat, LV_ALIGN_CENTER, 0, 20);
    lv_obj_set_size(sw_heat, 80, 40);

    lv_obj_add_event_cb(sw_heat, event_smart_control_cb, LV_EVENT_VALUE_CHANGED, NULL);
}
```

#### 3.3 创建干燥开关

```c
static void obj_create_sw_dry()
{
    lv_obj_t *sw_dry_parent = lv_obj_create(lv_scr_act());
    lv_obj_set_size(sw_dry_parent, 160, 160);
    lv_obj_align(sw_dry_parent, LV_ALIGN_CENTER, 230, 0);
    lv_obj_set_style_border_opa(sw_dry_parent, 70, 0); 

    // 创建标签
    lv_obj_t *label_dry = lv_label_create(sw_dry_parent);
    lv_label_set_text(label_dry, "Dry");
    lv_obj_align(label_dry, LV_ALIGN_CENTER, 0, -30);
    lv_obj_set_style_text_font(label_dry, &lv_font_montserrat_24, LV_STATE_DEFAULT);

    // 创建开关
    sw_dry = lv_switch_create(sw_dry_parent);
    lv_obj_align(sw_dry, LV_ALIGN_CENTER, 0, 20);
    lv_obj_set_size(sw_dry, 80, 40);
    lv_obj_add_state(sw_dry, LV_STATE_CHECKED | LV_STATE_DISABLED); // 设置为已开启且禁用状态

    lv_obj_add_event_cb(sw_dry, event_smart_control_cb, LV_EVENT_VALUE_CHANGED, NULL);
}
```

### 4. 创建标签

`obj_create_label` 函数创建一个标签，显示控制中心的标题。

```c
static void obj_create_label()
{
    lv_obj_t *label_control = lv_label_create(lv_scr_act());
    lv_label_set_text(label_control, "Control Center");
    lv_obj_align(label_control, LV_ALIGN_TOP_MID, 0, 40);
    lv_obj_set_style_text_font(label_control, &lv_font_montserrat_30, LV_STATE_DEFAULT);
}
```

### 5. 布局与交互

- **冷却、加热和干燥功能互斥**：通过在事件回调中清除不兼容开关的状态（例如，启用冷却时禁用加热）来实现这一点。
- **禁用干燥功能**：干燥功能在初始状态下被设置为开启且禁用，表示该功能不允许用户修改。
### 6. 总结

此示例通过 LVGL 创建了一个简单的智能家居控制系统，其中包含了冷却、加热和干燥三个开关，用户可以通过这些开关来控制不同的功能。开关的状态由回调函数进行管理，保证了不同功能之间的互斥性。通过这种方式，可以有效管理不同设备或功能的状态，使系统更具互动性和灵活性。
