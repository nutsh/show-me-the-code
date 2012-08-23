# nvm

nodejs 多版本管理工具: [nvm](https://github.com/creationix/nvm)

## 特点

* 简单方便
* 不污染系统环境
* bash_completion
* travis-ci.org 在使用

## 马上安装

clone 到你的目录

```
$ git clone git://github.com/creationix/nvm.git ~/nvm
```

添加到 ~/.bashrc or ~/.bash_profile 的最后一行

```
. ~/nvm/nvm.sh

# add bash_completion
[[ -r $NVM_DIR/bash_completion ]] && . $NVM_DIR/bash_completion

# set a default node version
nvm use 0.8
```

## 马上使用

Install nodev0.8.8 and nodev0.6.12

```
$ nvm install v0.8.8
$ nvm install v0.6.12
```

自由切换版本

```
$ nvm use 0.8
$ nvm use 0.6
```
