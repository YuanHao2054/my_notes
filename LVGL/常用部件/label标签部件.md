# LVGL 标签部件学习

标签部件用于显示文本，常见用途包括标题、提示等

## 标签部件的组成部分

- **主体 (`lv_part_main`)**：标签的主要部分，显示文本的区域。
- **滚动条 (`lv_part_scrollbar`)**：当文本超出容器时显示的滚动条。
- **选中的文本 (`lv_part_selected`)**：选中的文本部分。

---

## 学习目标

1. 创建标签部件并设置文本。
2. 修改文本样式。
3. 处理文本长度超出标签宽度的情况。
4. 为标签部件添加特殊效果 

---

## 1. 创建标签部件

通过以下代码可以在当前活动屏幕中创建一个标签，并将其居中对齐：

```c
lv_obj_t *label = lv_label_create(lv_scr_act());
lv_obj_align(label, LV_ALIGN_CENTER, 0, 0);
```

## 2. 设置文本的三种方式

- 动态设置文本

    ```c
    lv_label_set_text(label, "Hello world!Hello world!Hello world!Hello world!\nHello world!Hello world!Hello world!");
    ```
    此方式由 LVGL **动态分配内存**，推荐使用。

- 静态设置文本

    ```c
    lv_label_set_text_static(label, "Hello world!");
    ```

    文本存储在指定缓冲区中，适用于固定内容。

- 格式化文本输出

    ```c
    lv_label_set_text_fmt(label, "Hello %s!", "world");
    ```

支持类似 `printf` 的格式化输出。

## 3. 设置文本样式

### 3.1 全局样式设置

- **背景颜色**：
    ```c
    lv_obj_set_style_bg_color(label, lv_color_hex(0xECD664), LV_STATE_DEFAULT);
    ```

- **背景透明度**：
    ```c
    lv_obj_set_style_bg_opa(label, 100, LV_STATE_DEFAULT);
    ```

- **字体大小**：
    ```c
    lv_obj_set_style_text_font(label, &lv_font_montserrat_24, LV_STATE_DEFAULT);
    ```

    注意：使用的字体需在 `lv_conf.h` 中开启相应宏。

- **字体颜色**：
    ```c
    lv_obj_set_style_text_color(label, lv_color_hex(0x258EEA), LV_STATE_DEFAULT);
    ```

### 3.2 单独文本颜色设置

由于 v9 版本删除了 `lv_label_set_recolor` 函数，可以通过手动设置文本样式实现类似功能。详情参见 [GitHub Issue #5352](https://github.com/lvgl/lvgl/issues/5352)。  
- 启用重新着色 
    ```
    lv_label_set_recolor(label, true);
    ```
- 为指定的文字设置颜色  
    ```
    lv_label_set_text(label, "Hello #00ff00 world#");
    ```




## 4. 设置标签部件大小

标签部件默认大小根据文本内容自动调整，可以通过以下代码手动设置：

```c
lv_obj_set_size(label, 200, 60);
```

## 5. 处理长文本

当文本长度超过标签宽度时，可以设置不同的显示模式：


    lv_label_set_long_mode(label, LV_LABEL_LONG_SCROLL_CIRCULAR);


以下为可选模式：

| 模式                      | 描述                     |
|---------------------------|--------------------------|
| LV_LABEL_LONG_WRAP        | 默认模式，超出的文本会被裁剪。 |
| LV_LABEL_LONG_DOT         | 超出的文本替换为 ...。    |
| LV_LABEL_LONG_SCROLL      | 文本会来回滚动。         |
| LV_LABEL_LONG_SCROLL_CIRCULAR | 文本会循环滚动。       |
| LV_LABEL_LONG_CLIP        | 超出的文本被裁剪。       |
## 6. 文本对齐方式

- **文本对齐**（内容在标签内部的对齐方式）：
    ```c
    lv_obj_set_style_text_align(label, LV_TEXT_ALIGN_CENTER, 0);
    ```

- **标签部件对齐**（标签部件相对于父对象的对齐方式）：
    ```c
    lv_obj_align(label, LV_ALIGN_CENTER, 0, 0);
    ```

## 7. 特殊效果

- **阴影效果**：创建一个颜色不同且位置偏移的标签，实现阴影效果。
- **从其他标签部件获取文本**：
    ```c
    char *text = lv_label_get_text(label);
    lv_label_set_text(label2, text);
    ```

