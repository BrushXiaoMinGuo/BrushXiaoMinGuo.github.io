---
layout:       post
title:        "工具使用"
subtitle:     "tools"
date:         2018-06-05
author:       "Gxm"
header-img:   "img/project.jpg"
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - tools
---

###1: homebrew安装：
	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

###2: iterm安装
	brew cask install iterm2
	
###3: 配置iterm主题
	iTerm2 最常用的主题是 Solarized Dark theme，下载地址：http://ethanschoonover.com/solarized
	下载的是压缩文件，你先解压一下，然后打开 iTerm2，按Command + ,键，打开 Preferences 配置界面，然后Profiles -> Colors -> Color Presets -> Import，选择刚才解压的solarized->iterm2-colors-solarized->Solarized Dark.itermcolors文件，导入成功，最后选择 Solarized Dark 主题，就可以了。
	
###4: 配置zsh
	Oh My Zsh 是对主题的进一步扩展，地址：https://github.com/robbyrussell/oh-my-zsh
	一键安装：
	$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
	安装好之后，需要把 Zsh 设置为当前用户的默认 Shell（这样新建标签的时候才会使用 Zsh）：
	$ chsh -s /bin/zsh
	然后，我们编辑vim ~/.zshrc文件，将主题配置修改为ZSH_THEME="agnoster"。
	
	agnoster是比较常用的 zsh 主题之一，你可以挑选你喜欢的主题，zsh 主题列表：https://github.com/robbyrussell/oh-my-zsh/wiki/themes
	效果如下（配置了声明高亮）：
	
###5: 声明高亮：
	效果就是上面截图的那样，特殊命令和错误命令，会有高亮显示。
	使用 Homebrew 安装：
	$ brew install zsh-syntax-highlighting
	安装成功之后，编辑vim ~/.zshrc文件，在最后一行增加下面配置：
	source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
	
