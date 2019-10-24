# 前提

- 安装 [git](https://git-scm.com/downloads)
- 安装 [node](http://nodejs.cn/download/)

## 配置 iterm2

- [下载 iterm2](https://www.iterm2.com/downloads.html)，然后安装
- 安装 [oh-my-zsh](https://ohmyz.sh/)
- 安装字体 [Powerline fonts](https://github.com/powerline/fonts)
- 主题修改 [agnoster-zsh-theme](https://github.com/agnoster/agnoster-zsh-theme)
  `~/.zshrc` -> `ZSH_THEME="agnoster"`
- 修改颜色，字体“powerline”

## 安装 sublime

- [看以前的文章](https://github.com/hangyangws/notes/blob/master/src/sublime.md)

## 安装 vscode

- 下载：https://code.visualstudio.com/

- 安装插件:

1. markdownlint
2. Sublime Text Keymap and Settings Importer
3. 修改 terminal 字体为 powerline: `"terminal.integrated.fontFamily": "Source Code Pro for Powerline"`
4. cmd + k 清理 terminal 屏幕

  按住 cmd+Shift+P组合按钮打开设置窗口，输入> open keyboard Shortcuts(JSON), 在里面输入:

  ```json
  [{

    "key": "ctrl+k",
    "command": "workbench.action.terminal.clear",
    "when": "terminalFocus"
  }]
  ```

5. 支持 code 命令行打开 vsCode:

  ⌘⇧P, 输入 ‘shell command’，在提示里看到 Shell Command: Install 'code' command in PATH，运行它就可以了
