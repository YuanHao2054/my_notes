## 下拉列表（lv_dropdown）简介

`lv_dropdown` 是一种常用于多选一场景的组件，通过点击可展示多个选项供用户选择。

### 组成部分

- **按钮（Button）**：默认显示内容的部分，点击后显示完整选项。
- **列表（List）**：点击按钮后展开的选项列表。
## 示例代码解析与学习

### 1. **创建下拉列表**

```c
dropdown = lv_dropdown_create(lv_scr_act());
lv_obj_set_size(dropdown, 200, 50); // 设置下拉列表大小
lv_obj_align(dropdown, LV_ALIGN_CENTER, 0, 0); // 居中对齐
```

**核心功能**：

- 创建一个下拉列表对象，设置大小和位置。
### 2. **设置字体**

```c
lv_obj_set_style_text_font(dropdown, &lv_font_montserrat_24, 0); 
```

**说明**：此代码仅影响默认按钮中的字体大小，不会改变选项列表中的字体。

### 3. **设置选项内容**

```c
lv_dropdown_set_options(dropdown, "Apple\nBanana\nOrange\nMelon\nGrape\nRaspberry");
```

- **设置选项**：通过字符串形式设置，每个选项用 \n 分隔。
- **内存优化（静态选项）**：
```c
lv_dropdown_set_options_static(dropdown, "a\nb\nc\nd");
```
### 4. **动态添加选项**

```c
lv_dropdown_add_option(dropdown, "Kiwi", 2); // 在索引 2 处插入选项 Kiwi
```

- 可通过索引指定选项插入的位置（索引从 0 开始）。
- 宏 LV_DROPDOWN_POS_LAST 可用于将选项添加到末尾。
### 5. **设置默认选项**

```c
lv_dropdown_set_selected(dropdown, 2); // 默认选择第 3 项（索引 2）
```

- 使用索引值设置默认选项。
### 6. **获取选中的选项**

以下两种方法可用于获取当前选中的选项：

1. **获取索引值**：
```c
lv_dropdown_get_selected(dropdown);
```
2. **获取选项字符串**：
```c
char buf[32];
lv_dropdown_get_selected_str(dropdown, buf, sizeof(buf));
printf("当前选项是：%s\n", buf);
```
### 7. **设置下拉列表方向与图标**

```c
lv_dropdown_set_dir(dropdown, LV_DIR_BOTTOM); // 下拉方向为向下
lv_dropdown_set_symbol(dropdown, LV_SYMBOL_DOWN); // 使用系统图标作为按钮标识
```

- **方向**：支持向上、向下、向左、向右。
- **图标**：可使用 LVGL 内置的图标枚举。
### 8. **回调函数**

```c
static void event_dropdown_cb(lv_event_t *e)
{
    char buf[32];
    lv_dropdown_get_selected_str(dropdown, buf, sizeof(buf));
    printf("选中的选项是：%s\n", buf); // 打印选中选项的文本内容
    printf("选中的索引是：%d\n", lv_dropdown_get_selected(dropdown)); // 打印选中选项的索引
}
lv_obj_add_event_cb(dropdown, event_dropdown_cb, LV_EVENT_VALUE_CHANGED, NULL);
```

- **功能**：当用户选择选项时，触发事件回调，获取选中选项的内容或索引。
