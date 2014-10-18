---
layout: post
title: "getting started with emacs"
date: 2014-07-23 10:10:11 +0800
comments: true
categories: emacs
---
##Why emacs?
Previously I was developing with dynamic language, like js, ruby, python, via using vim. Vim is a great editing tools with different internal mode(command/visial/insert mode). But the original reason why I intended to migrate from vim to emacs is the powerful `REPL` environment as well as other embedded function.
##Trouble from vim to emacs
I was starting to read the documentation of emacs, and acquainted with new key-binding. Then I realized editing in emacs is less effective than in vim. Thus I came up to `evil`, an extensible vi layer for Emacs. Combing emacs with evil will maximize the productivies, therefore it is not necessary for you to remember all the key-binding under emacs environment.

<!--more-->

##How to install on OS X?
Download and install emacs, no args will not install gui enviornment.

```
brew install --cocoa emacs
brew install --cocoa --srgb emacs
vim .bash_profile
alias emacs="/usr/local/Cellar/emacs/24.3/bin/emacs-24.3 -nw"

ln -s /usr/local/Cellar/emacs/24.3/Emacs.app /Applications
```
##Keyboard setting on OS X
1. open the terminal perference, and then set option as a meta key.
2. recommend you to swap capslock with ctrl

##Basic command you should remember
Emacs self key-binding

* `M-x help-with-tutorial` open emacs tutorial
* `C-x C-c` quit
* `C-x C-f` open file
* `C-x C-s` save file
* `C-g` quit a partially entered command
* `C-h m` list active mode

* `C-x left-arrow` previous buffer
* `C-x C-b` list buffers
* `M-x desktop-clear`

* `C-x 0` close current window
* `C-x 1` close other window except current
* `C-x 2` split window vertically
* `C-x 3` split window horizontally
* `C-x 4 C-f` open new window and move the cursor to it

* `C-v` scroll down the window
* `M-v` scroll up the window
* `C-M-v` scroll the botton window without focusing the cursor

* `M-;` comment on current line

Plugin default setting key-binding

* `tab` autocomplete in evil-insert mode
* `gv` reselect evil visual mode"
* `:%s/foo/bar/g` replace all matched words in evil-normal mode
* `1g` or `ng` jump to the specific line
* `C-c b` send buffer to REPL
* `C-c r` send region to REPL
* `C-c f` format js/html/css by web-beautify


##Emacs package system
There are four package servers, including GUN, ELPA, Marmalade, MELPA. Currently I choosed MELPA as a default package system, becasue I forked config from [Steve Purcell emacs.d repository](https://github.com/purcell/emacs.d/ "Title").

There are several starter kits you can begin with:

1. [emacs24-starter-kit](https://github.com/eschulte/emacs24-starter-kit) is highly recommended if you want a clean environment and build it from scratch.
2. [emacs prelude](https://github.com/bbatsov/prelude) is not recommended by novice.
3. [Steve Purcell's emacs.d](https://github.com/purcell/emacs.d).
4. [oh-my-emacs](https://github.com/xiaohanyu/oh-my-emacs) adapted el-get instead of the builtin package.el.
5. [redguardtoo's emacs](https://github.com/redguardtoo/emacs.d) is forked from Purcell's emacs.d, and make it localize if you have no access to network. In addition, it enhances some features on the top of Purcell's config.

Steve Purcell emacs config is really impressive and the structure is clean and tidy. So I highly recommmend you to fork his repos and localize your configure file into your home directory.

Note that if you encounter any difficulties, please run command `emacs --debug-init` to check any error.
##Increamtal plugin list besides purcell config
1. [evil](https://gitorious.org/evil/pages/Home) is an extensible vi layer for Emacs.
2. [helm](https://github.com/emacs-helm/helm) is Emacs incremental and narrowing framework.
3. [ac-helm](https://github.com/yasuyk/ac-helm) integrates AC in helm mode.
4. [yasnippet](https://github.com/capitaomorte/yasnippet) is a template system for Emacs. The snippet syntax is inspired from TextMate's syntax.
5. [ace-jump-mode](https://github.com/yasuyk/ac-helm) is A quick cursor location minor mode for emacs, like easymotion under vim.
6. [web-beautify](https://github.com/yasuyk/web-beautify) is used to format HTML, CSS and JavaScript/JSON by js-beautify

##Other notes
1. Please `brew install ispell` before enabling flyspell
2. Using `node --interactive` as inferior-js-program-command in js-comint mode.
3. Please `(setq auto-save-default nil)` in order to disable auto-save-mode.
4. If you cannot download plugins, please `M-x package-list-packages` to refresh your package list
##Reference
1. http://ergoemacs.org/misc/list_of_emacs_starter_kits.html
2. http://ergoemacs.org/emacs/emacs_package_system.html
3. http://batsov.com/articles/2012/02/19/package-management-in-emacs-the-good-the-bad-and-the-ugly/
4. http://toumorokoshi.github.io/emacs-from-scratch-part-1-extending-emacs-basics.html
5. http://blog.csdn.net/redguardtoo/article/details/7222501
