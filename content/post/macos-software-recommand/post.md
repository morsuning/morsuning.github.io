---
title: "MacOS 开发软件及 Shell 配置推荐"
date: 2024-05-11
author: morsuning
categories: ["Share"]
tags: ["R&D Efficiency"]
---
## 常用软件

### 软件来源

[macwk](https://macwk.cn/) 学习版MacOS软件下载

Mac App Store 苹果官方应用商店，方便下载、卸载、更新和付费

Homebrew MacOS软件包管理器，通常用来安装开源软件

各软件官方网站

### 必备软件

[applite](https://github.com/milanvarady/Applite) brew GUI 建议在此安装推荐的软件和任何brew安装的软件，方便管理

appcleaner App卸载工具

[clash-verge-rev](https://github.com/clash-verge-rev/clash-verge-rev) | [hiddify-next](https://github.com/hiddify/hiddify-next) 科学上网工具

The Unarchiver 压缩工具

item2 | wrap 终端模拟器

edge | arc 浏览器

motrix 全功能下载管理器

VSCode 全能代码编辑器

欧陆词典 单词查询&翻译

Notion 在线笔记 | Anytype 本地+在线笔记

Obsidian 离线可同步笔记

Microsoft Office 微软全家桶，办公专用

[SnippetsLab](http://www.renfei.org/snippets-lab/) | Lepton 优秀的代码片段管理工具, 轻量, 可基于菜单栏操作

reeder RSS订阅浏览器

iina or infuse 视频播放器

koodo Reader 电子书管理，可替代calibre，支持多设备同步及epub等多种电子书格式

[dash](https://kapeli.com/dash) 开发文档查阅

[buhontfs](https://www.drbuho.com/buhontfs) NTFS磁盘工具，可读写NTFS磁盘，我是免费期入的，可用其他软件替代

[stats](https://github.com/exelban/stats) iStats开源平替，菜单栏系统资源监视器

[tailscale](https://tailscale.com/) 内网穿透工具

raycast | utools 效率工具平台，可集成各类效率插件，补全系统级功能，[raycast 插件推荐](https://github.com/marekbrze/categorized-raycast-extensions)

Barbee | Bartender 菜单栏项自定义隐藏，避免被刘海遮挡

PDF Expert pdf编辑工具

XnViewMP 图片查看格式转换压缩

rustdesk 远程桌面工具

whisky 运行windows应用

playcover 运行未在Mac App Store上架的ios应用

UTM | Parallels Desktop 虚拟机软件

Quick Look 插件: 选中文件，按空格预览

- QuicklookStephen - 查看未知拓展名的纯文本文件 brew install --cask qlstephen
- QLMarkdown - 空格键预览 Markdown 文本效果 - brew install --cask qlmarkdown

### 可选软件

[capslock](https://capslock.vonng.com/#/zh-cn/) 将CapsLock键改造为强力功能修饰键

Downie 下载视频，也可以使用[cobalt](https://cobalt.tools/)这个网站

switchhosts host切换

[KeyCastr](http://mac.softpedia.com/get/Utilities/KeyCastr.shtml) 将mac按键显示在屏幕上，分享演示、录制视频或动图时超赞

pixpin 截图&OCR工具

QSpace & TotalFinder 访达增强

BetterMouse 全能鼠标驱动设置

permute3 格式转换

android file transfer 传输安卓系统文件

Etcher 制作 U 盘镜像

ventoy 新一代多系统启动U盘解决方案

CheatSheet 按command显示当前应用快捷键

AlDente 用于MacBooks的ALL-IN-ONE充电限制器应用软件

**开发者工具**

DBeaver ｜ Naviat 数据库客户端

postman API管理工具

[chsrc](https://github.com/RubyMetric/chsrc) 全平台一件换源工具

### 命令行工具简要推荐

wget 下载工具

tmux 终端复用

cloc 代码行数统计命令行工具

**proxychains-ng** 终端命令行下代理神器，可以让指定的命令走设置好的代理，内网渗透、科学上网必备工具 brew install proxychains-ng 配置文件: /usr/local/etc/proxychains.conf

- proxychains4 curl https://www.google.com.hk 通过代理访问
- proxychains4 -q /bin/zsh 开启zsh全局代理
- 配置文件最后一行配置示例: http 127.0.0.1 7897
- 建议配置alias pc=’proxychains4’

## Shell

使用当前MacOS下体验最佳终端Warp，搭配MacOS自带的zsh及其配置框架oh-my-zsh

先依次安装

1. oh-my-zsh: zsh配置框架 sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
2. brew: 包管理工具

### 配置oh-my-zsh

修改～/.zshrc

**主题**

ZSH_THEME=”robbyrussell”默认

推荐主题：简洁 - cloud, af-magic;详细 - ys, dst

**zsh插件**

- 自动跳转插件autojump

```bash
git clone https://github.com/wting/autojump.git $ZSH_CUSTOM/plugins/autojump;
cd $ZSH_CUSTOM/plugins/autojump;
python3 install.py;
```

只要访问过某个目录直接 j 目录 即可跳转到该目录

- extract (oh-my-zsh内置)一个命令解压所有文件 x <文件名> 即可解压该文件
- zsh-syntax-highlighting 语法高亮插件

```jsx
git clone https://github.com/zsh-users/zsh-syntax-highlighting $ZSH_CUSTOM/plugins/zsh-syntax-highlighting;
```

- zsh-autosuggestions 命令自动补全插件

```jsx
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions;
```

**最终的参考.zshrc文件**

```bash

# If you come from bash you might have to change your $PATH.
export PATH=$HOME/env/bin:$HOME/.local/bin:/usr/local/bin:/opt/homebrew/bin:$PATH

# Homebrew mirror
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"
export HOMEBREW_API_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles/api"

# Path to your oh-my-zsh installation.
export ZSH="$HOME/.oh-my-zsh"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
ZSH_THEME="ys"

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME=random will cause zsh to load
# a theme from this variable instead of looking in $ZSH/themes/
# If set to an empty array, this variable will have no effect.
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment one of the following lines to change the auto-update behavior
# zstyle ':omz:update' mode disabled  # disable automatic updates
# zstyle ':omz:update' mode auto      # update automatically without asking
# zstyle ':omz:update' mode reminder  # just remind me to update when it's time

# Uncomment the following line to change how often to auto-update (in days).
# zstyle ':omz:update' frequency 13

# Uncomment the following line if pasting URLs and other text is messed up.
# DISABLE_MAGIC_FUNCTIONS="true"

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# You can also set it to another string to have that shown instead of the default red dots.
# e.g. COMPLETION_WAITING_DOTS="%F{yellow}waiting...%f"
# Caution: this setting can cause issues with multiline prompts in zsh < 5.7.1 (see #5765)
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# You can set one of the optional three formats:
# "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# or set a custom format using the strftime function format specifications,
# see 'man strftime' for details.
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git extract textmate ruby mvn gradle autojump zsh-syntax-highlighting zsh-autosuggestions)

source $ZSH/oh-my-zsh.sh

# User configuration
autoload -U compinit

# export MANPATH="/usr/local/man:$MANPATH"

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"
alias dk="docker"
alias python="python3"
alias cls='clear'
alias vi='vim'
alias pc='proxychains4'
alias javac="javac -J-Dfile.encoding=utf8"
alias grep="grep --color=auto"
alias -s html=vi # 在命令行直接输入后缀为 html 的文件名，会在 TextMate 中打开
alias -s rb=vi # 在命令行直接输入 ruby 文件，会在 TextMate 中打开
alias -s py=vi # 在命令行直接输入 python 文件，会用 vim 中打开，以下类似
alias -s js=vi
alias -s c=vi
alias -s java=vi
alias -s txt=vi
alias -s gz='tar -xzvf'
alias -s tgz='tar -xzvf'
alias -s zip='unzip'
alias -s bz2='tar -xjvf'

# Source global definitions
s2c () { scp "$@" ${SSH_CLIENT%% *}:~/; }
c2s () { scp ${SSH_CLIENT%% *}:"$@" .; }
```
