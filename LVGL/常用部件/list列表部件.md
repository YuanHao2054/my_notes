# LVGL 列表部件学习笔记

## 列表组件的基本介绍

### 组成部分
- **主体 (lv_part_main)**: 列表的背景部分。
- **滚动条 (lv_part_scrollbar)**: 用于显示和控制滚动。

### 列表的特点
- 列表通常用于多选一的功能，能够显示多个选项。
- 列表的每个按钮可以设置图标和文本。

## 列表创建的基本步骤

### 创建列表和设置属性
1. 使用 `lv_list_create` 创建列表对象，并设置其大小和对齐方式。
2. 使用 `lv_list_add_text` 添加标题文本。
3. 设置列表主体的样式，如字体大小（`lv_obj_set_style_text_font`）。

### 添加按钮
- 使用 `lv_list_add_btn` 方法为列表添加按钮。
  - 参数说明：
    1. 列表对象。
    2. 按钮的图标（可以使用 LVGL 内置图标枚举，如 `LV_SYMBOL_OK`）。
    3. 按钮的文本。
- 为按钮添加事件回调函数（如 `LV_EVENT_CLICKED`）。

### 获取按钮文本
- 使用 `lv_list_get_btn_text` 方法获取按钮的文本内容，通常在事件回调中使用。

### 添加事件
- 为列表中的按钮绑定事件回调函数（如 `lv_obj_add_event_cb`）。

## 列表部件示例代码分析

### 示例代码1：基本列表创建
```c
lv_obj_t *list1 = lv_list_create(lv_scr_act());
lv_obj_align(list1, LV_ALIGN_CENTER, 0, 0);
lv_obj_set_size(list1, 200, 200);
lv_list_add_text(list1, "Setting");
lv_obj_set_style_text_font(list1, &lv_font_montserrat_14, LV_PART_MAIN);

lv_obj_t *btn1 = lv_list_add_btn(list1, LV_SYMBOL_OK, "Ok");
lv_obj_add_event_cb(btn1, event_list_cb, LV_EVENT_CLICKED, NULL);
```
#### 代码解读
1. 创建一个列表 `list1`，并设置大小和居中对齐。
2. 添加标题 "Setting"，并设置主体部分的字体为 `lv_font_montserrat_14`。
3. 添加一个按钮，按钮的图标为 "OK" 图标，文本为 "Ok"。
4. 为按钮添加点击事件回调函数。

### 示例代码2：复杂列表练习
#### 代码
```c
static void event_list_cb(lv_event_t *e)
{
    lv_obj_t *target = lv_event_get_target(e);
    const char *text = lv_list_get_btn_text(lv_obj_get_parent(target), target);
    lv_label_set_text_fmt(label, "%s", text);
    printf("Button %s is clicked\n", text);
    lv_obj_add_state(target, LV_STATE_FOCUS_KEY);
}

static void obj_left()
{
    lv_obj_t *obj = lv_obj_create(lv_scr_act());
    lv_obj_set_size(obj, 300, 400);
    lv_obj_align(obj, LV_ALIGN_CENTER, -200, 0);

    lv_obj_t *list = lv_list_create(obj);
    lv_obj_align(list, LV_ALIGN_CENTER, 0, 0);
    lv_obj_set_size(list, 250, 300);
    lv_obj_set_style_text_font(list, &lv_font_montserrat_14, LV_PART_MAIN);

    lv_list_add_text(list, "File");
    btn = lv_list_add_btn(list, LV_SYMBOL_FILE, "File");
    lv_obj_add_event_cb(btn, event_list_cb, LV_EVENT_CLICKED, NULL);
    btn = lv_list_add_btn(list, LV_SYMBOL_DIRECTORY, "Directory");
    lv_obj_add_event_cb(btn, event_list_cb, LV_EVENT_CLICKED, NULL);
    btn = lv_list_add_btn(list, LV_SYMBOL_CLOSE, "Close");
    lv_obj_add_event_cb(btn, event_list_cb, LV_EVENT_CLICKED, NULL);

    lv_list_add_text(list, "Connectivity");
    btn = lv_list_add_btn(list, LV_SYMBOL_WIFI, "WiFi");
    lv_obj_add_event_cb(btn, event_list_cb, LV_EVENT_CLICKED, NULL);
    btn = lv_list_add_btn(list, LV_SYMBOL_BLUETOOTH, "Bluetooth");
    lv_obj_add_event_cb(btn, event_list_cb, LV_EVENT_CLICKED, NULL);
}

static void obj_right()
{
    lv_obj_t *obj = lv_obj_create(lv_scr_act());
    lv_obj_set_size(obj, 300, 400);
    lv_obj_align(obj, LV_ALIGN_CENTER, 200, 0);

    label = lv_label_create(obj);
    lv_obj_align(label, LV_ALIGN_CENTER, 0, 0);
    lv_label_set_text_fmt(label, "Hello World");
    lv_obj_set_style_text_font(label, &lv_font_montserrat_30, 0);
}

void mygui(void)
{
    obj_left();
    obj_right();
}
```
#### 代码解读
1. **左侧组件**：
   - 创建一个父对象 `obj`，设置大小和对齐。
   - 创建一个列表对象 `list`，并添加两个分组："File" 和 "Connectivity"。
   - 在每个分组下添加多个按钮，并为每个按钮绑定 `event_list_cb`。
2. **右侧组件**：
   - 创建一个父对象 `obj`，用于放置标签。
   - 创建标签对象 `label`，初始化为 "Hello World"，并设置字体大小。
3. **事件回调函数 `event_list_cb`**：
   - 获取按钮文本内容，并在右侧标签中显示对应文本。
   - 打印点击信息到终端。
   - 给选中的按钮添加聚焦状态。

### 新增知识点
- **分组和分隔符**：
  - 使用 `lv_list_add_text` 可以在列表中添加分隔符，帮助逻辑分组。
- **状态管理**：
  - 使用 `lv_obj_add_state` 可以为对象添加不同的状态（如 `LV_STATE_FOCUS_KEY`）。
- **实时交互**：
  - 按钮点击后，可以通过回调函数实现动态交互。

## 练习总结
通过该练习，进一步巩固了以下内容：
1. **列表的基础操作**：列表的创建、按钮的添加和分组。
2. **事件回调的应用**：根据按钮的点击事件实现动态交互。
3. **多组件协作**：列表与标签组件协作显示选中内容。

## 提升方向
- 可以尝试使用更多复杂的样式调整，如滚动条样式。
- 学习如何动态添加或删除按钮。
- 优化事件回调函数，使其更加通用和高效。

