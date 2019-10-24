# sublime 配置 & 教程

> 主要是真对前端开发友好的配置

### 下载 sublime

[官网下载](https://www.sublimetext.com/3)

### 安装 `Package Control`

快捷键「ctrl + \`」，然后输入对应的 Python code，详情见 [官网链接](https://packagecontrol.io/installation)。

### 安装需要的 `Package`

1. 快捷键 `command + shift + P`
1. 输入 `Install Package`
1. 回车，等待一会儿
1. 输入想要安装的 `Package`

安利一些前端合适的 `Package`「具体 Package 的作用可以自己 google」：

- Babel
- CSScomb
- DocBlockr
- Emmet
- HTML-CSS-JS Prettify
- LESS
- Meterial Theme
- Sass
- SideBarEnhancements
- TrailingSpaces

### 卸载 `Package`

1. 快捷键 `command + shift + P`
1. 输入 `Remove Package`
1. 输入想要卸载的 `Package`，回车

### 配置 sublime 参数

快捷键 `command + shift + P`，输入 `Settings`，回车；  
或者直接输入快捷键 「command + \`」。
然后在弹出的窗口右侧输入配置。

以下为建议配置「对应的含义请自行 google」：

```json
{
  "always_show_minimap_viewport": true,
  "caret_style": "phase",
  "css":
  {
    "indent_char": " ",
    "indent_size": 2
  },
  "enable_tab_scrolling": false,
  "folder_exclude_patterns":
  [
    ".svn",
    ".git",
    "node_modules",
    ".hg",
    "CVS"
  ],
  "font_options":
  [
    "gray_antialias",
    "subpixel_antialias"
  ],
  "font_size": 14,
  "highlight_line": true,
  "highlight_modified_tabs": true,
  "html":
  {
    "indent_char": " ",
    "indent_size": 2
  },
  "ignored_packages":
  [
    "Vintage"
  ],
  "indent_guide_options":
  [
    "draw_normal",
    "draw_active"
  ],
  "js":
  {
    "indent_char": " ",
    "indent_size": 2
  },
  "line_padding_bottom": 3,
  "line_padding_top": 3,
  "overlay_scroll_bars": "enabled",
  "rulers":
  [
    80
  ],
  "scroll_past_end": true,
  "show_full_path": true,
  "tab_completion": true,
  "tab_size": 2,
  "translate_tabs_to_spaces": true,
  "vintage_start_in_command_mode": true,
  "word_wrap": true
}
```

### 配置 sublime 主题

个人审美不同，我喜欢：Material Theme  
Material Theme 的 [github 首页](https://github.com/equinusocio/material-theme#easy-installation) 有安装教程  
安装成功后，在 sublime 参数里面添加如下配置  

```json
{
  "theme": "Material-Theme.sublime-theme",
  "color_scheme": "Packages/Material Theme/schemes/Material-Theme.tmTheme",
  "material_theme_compact_sidebar": true,
  "material_theme_contrast_mode": true,
  "material_theme_small_tab": true,
  "material_theme_tabs_autowidth": true,
}
```

「除了第一行 `"theme": "Material-Theme.sublime-theme"` 是必须的配置以外，  
其他的都是自定义配置，可以根据 [官方配置表](https://github.com/equinusocio/material-theme#theme-options) 选择」
