---
layout: post
title: "Install Ruby Vim Command without Root"
date: 2014-05-30 08:17:24 +0800
comments: true
categories: ruby
---
This post intend to write down the step for installing ruby and vim command line tools, without root access.
In addition, ncurses is the prerequisities of installing ruby and vim. Also, please install ruby first in order to enable rubyinterp.

<!--more-->

1. download ncurses
2. `tar xvf ncurses-5.9.tar.gz`
3. sh `./configure --prefix=/home/bobby/tools/ncurses ##--with-shared`
4. `make`
5. `make install`
6. download ruby
7. sh `./configure --prefix=/home/bobby/tools/ruby1.9 --with-tlib=ncurses`
8. `make`
9. `make install`
10. download from vim
11. `bzip2 -cd vimxxx.tar.bz2 | tar xvf -`
12. edit .bash_profile `set CPPLAGS="-I/home/bobby/tools/ncurses/include" LDFLAGS="-L/home/bobby/tools/ncurses/lib" export CPPFLAGS LDFLAGS`
13. sh `./configure --prefix=/home/bobby/tools/vim7.4 --disable-selinux --enbale-gui=no --without-x --disable-gpm --disable-nls --with-tilib=ncurses --enable-multibyte --enable-rubyinterp --enable-perlinterp --enable-pythoninterp`
14. `make`
15. `make install`
16. edit ~/.bashrc `alias vim="/home/bobby/tools/vim7.4/bin/vim"`
17. edit ~/.vimrc `set synatx on set nocompatible set backspace=2`
18. setup vimruntime in order to let vim find the syntax env, please edit .bash_profile `export VIMRUNTIME=/home/bobby/tools/preinstall/vim74/runtime`