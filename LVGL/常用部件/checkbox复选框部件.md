 ## LVGL 复选框部件学习

### 1. 复选框部件概述

复选框（Checkbox）是 LVGL 提供的一个 UI 组件，通常用于表示用户选择或不选择某个选项。它包含两个主要部分：

- **主体（lv_part_main）**：复选框的整体外观和布局。
- **勾选框（lv_part_indicator）**：实际的勾选框部分，显示勾选状态。
### 2. 复选框的创建与布局

在 LVGL 中，复选框的创建通过 `lv_checkbox_create` 函数进行。以下是创建和对齐复选框的代码：

```c
// 创建一个复选框
checkbox1 = lv_checkbox_create(lv_scr_act());

// 将复选框对齐到屏幕中心
lv_obj_align(checkbox1, LV_ALIGN_CENTER, 0, 0);

// 可选：设置复选框的大小（此行代码被注释掉了）
lv_obj_set_size(checkbox1, 100, 50);
```

- lv_checkbox_create：创建一个新的复选框对象，lv_scr_act() 表示当前活动屏幕，即复选框会被添加到当前活动的屏幕上。
- lv_obj_align：将复选框对齐到屏幕的中央。
### 3. 设置复选框的文本

复选框可以显示文本，通常用来描述复选框的功能或选项。

```c
// 设置复选框的文本
lv_checkbox_set_text(checkbox1, "Checkbox1");
```

- lv_checkbox_set_text：设置复选框旁边显示的文本，这里设置为 "Checkbox1"。
### 4. 设置文本样式

可以通过 `lv_obj_set_style_*` 系列函数对复选框的文本进行样式设置。例如，设置文本与复选框之间的间距和文本的字体。

```c
// 设置文本和复选框之间的间距
lv_obj_set_style_pad_column(checkbox1, 20, 0);

// 设置文本的字体
lv_obj_set_style_text_font(checkbox1, &lv_font_montserrat_24, LV_STATE_DEFAULT);
```

- lv_obj_set_style_pad_column：设置文本与复选框之间的间距。
- lv_obj_set_style_text_font：设置文本的字体。此处使用 lv_font_montserrat_24 字体。
### 5. 设置复选框的状态

复选框有两种基本状态：选中（`LV_STATE_CHECKED`）和未选中。可以通过以下方法设置或清除复选框的状态：

```c
// 设置为选中状态
lv_obj_add_state(checkbox1, LV_STATE_CHECKED);

// 可选：清除选中状态，设置为未选中
lv_obj_clear_state(checkbox1, LV_STATE_CHECKED);
```

- lv_obj_add_state：将复选框设置为选中状态。
- lv_obj_clear_state：清除选中状态，设置为未选中（此行代码被注释掉了）。
### 6. 获取复选框的状态

可以使用 `lv_obj_has_state` 函数来获取复选框当前的状态，判断是否被选中。

```c
// 获取复选框的状态，判断是否为选中状态
bool state = lv_obj_has_state(checkbox1, LV_STATE_CHECKED);
printf("checkbox1 state: %d\n", state);
```

- lv_obj_has_state：判断复选框是否处于选中状态。返回 true 表示选中，false 表示未选中。
### 7. 复选框状态变化事件

复选框的状态变化（选中与未选中之间的切换）通常会触发一个事件。我们可以通过事件回调函数来响应这些变化。以下代码演示了如何为复选框添加事件回调：

```c
// 添加事件回调，当复选框的值发生变化时触发
lv_obj_add_event_cb(checkbox1, event_checkbox_cb, LV_EVENT_VALUE_CHANGED, NULL);
```

- lv_obj_add_event_cb：将回调函数 event_checkbox_cb 与复选框的状态变化事件（LV_EVENT_VALUE_CHANGED）关联。当复选框的值发生变化时，回调函数会被调用。
### 8. 事件回调函数

回调函数用于处理复选框状态变化时的逻辑。在这个回调函数中，我们获取复选框的状态并打印：

```c
static void event_checkbox_cb(lv_event_t *e)
{
    // 获取触发事件的目标对象（复选框）
    lv_obj_t *target = lv_event_get_target(e);
    lv_event_code_t code = lv_event_get_code(e);

    // 如果事件源是 checkbox1
    if (target == checkbox1)
    {
        // 检查事件类型是否为状态变化事件
        if (code == LV_EVENT_VALUE_CHANGED)
        {
            // 获取复选框的状态
            bool state = lv_obj_has_state(checkbox1, LV_STATE_CHECKED);
            printf("checkbox1 state: %d\n", state); // 打印复选框的状态
        }
    }
}
```

- lv_event_get_target：获取事件源，即触发事件的对象。
- lv_event_get_code：获取事件的类型代码（此处为 LV_EVENT_VALUE_CHANGED，即状态变化事件）。
- lv_obj_has_state：再次用于获取复选框的状态。
