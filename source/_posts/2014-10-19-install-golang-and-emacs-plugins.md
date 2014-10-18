---
layout: post
title: install golang and emacs plugins
date: 2014-10-19 01:09:56
categories: golang
tags:
---

##env setup
* create dir `GOPATH=~/Projects/goprojects`
* edit .bash_profile
```
GOPATH=~/Projects/goprojects
GOROOT=/usr/local/go
GOOS=darwin
GOARCH=amd64
PATH=$PATH:$GOROOT/bin:$GOPATH/bin
export PATH
export GOPATH GOOS GOARCH
```

##emac plugins tips
* copy `/usr/local/go/misc/emacs/go-mode-load.el` into emacs lisp dir, and make minor modification on your own personal setting
* install [mercurial](http://mercurial.selenic.com/downloads), otherwise you cannot run hg command
* `go get code.google.com/p/rog-go/exp/cmd/godef`, then we can use godef function
* follow steps with [nsf/gocode](https://github.com/nsf/gocode/blob/master/README.md)


##Plugins list
1. https://github.com/dominikh/go-mode.el
2. https://github.com/nsf/gocode

##References
1. http://dominik.honnef.co/posts/2013/03/writing_go_in_emacs/
2. http://tleyden.github.io/blog/2014/05/22/configure-emacs-as-a-go-editor-from-scratch/
3. https://golang.org/doc/install#download
4. http://go-lang.cat-v.org/text-editors/