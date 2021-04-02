---
title: mac first met
date: 2021-01-21
draft: false
---

# 听说你买了 Mac

## 背景

听说很多同学买了 Mac，所以简单罗列下平常使用到的一些工具，可以参考参考。

## 工具介绍

### homebrew

> - 介绍
>   - Mac 下安装工具或软件的必备工具[^1]
> - 如何安装
>   - `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
> - 如何使用
>   - `brew install wget`
>   - `brew cask install chrome`


### iTerm2

> - 介绍
>   - 替换 Mac 自带的终端
> - 如何安装
>   - 官网下载[^2]
> - 如何使用
>   - 推荐搭配`oh-my-zsh`使用


### oh-my-zsh

> - 介绍
>   - zsh 配置管理工具
> - 如何安装
>   - `sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
> - 必装插件
>   - autojump[^3]
>   - fzf[^4]
>   - git
>   - docker


### Alfred

> - 介绍
>   - 效率神器
> - 如何安装
>   - 官网下载[^5]
> - 如何使用
>   - 基本使用
>       - 找文件
>       - 搜`Google`
>       - 当计算器用
>   - 进阶使用
>       - 结合`workflow`使用
>       - 自定义`workflow`

推荐的一些`workflow`

- dash(安装Dash后，在Integration里打开Alfred)
- jetBrains[^6]
- kill[^7]
- stack overflow[^8] 👼

### 还有...

- 录屏神器 GIPHY CAPTURE[^9]
- Amphetamine(安非他命)
- Markdown写作工具 MWeb[^10]


## 参考资料

- [Mac 入门指南 2.0](https://zhuanlan.zhihu.com/p/83863239)
- [Mac 下有哪些能极大地提高工作效率的软件？](https://www.zhihu.com/question/27158546)
- [MacTalk](http://macshuo.com/?tag=mac)
- [alfred-workflows](https://github.com/zenorocha/alfred-workflows)


## 引用

[^1]: https://brew.sh/
[^2]: https://iterm2.com/
[^5]: https://www.alfredapp.com/
[^9]: https://giphy.com/apps/giphycapture/
[^8]: https://github.com/zenorocha/alfred-workflows/raw/master/stack-overflow/stack-overflow.alfredworkflow
[^6]: https://bchatard.github.io/jetbrains-alfred-workflow/
[^7]: https://github.com/ngreenstein/alfred-process-killer/
[^3]: https://github.com/wting/autojump/
[^4]: https://github.com/junegunn/fzf/
[^10]: https://zh.mweb.im/