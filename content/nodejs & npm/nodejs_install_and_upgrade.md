---
title: "nodejs 安装与升级"
layout: page
date: 2016-09-22 00:00
---

[TOC]

### ubuntu 安装 nodejs ###
```shell
sudo apt-get install nodejs

sudo apt-get install nodejs-legecy

sudo apt-get install npm
```
### 升级npm
```shell
sudo npm install -g npm
sudo npm install -g npm （多重复一次）
```

### 通过npm来升级nodejs
```shell
node -v
sudo npm cache clean -f

Install n

sudo npm install -g n

sudo n 0.8.11

sudo n stable

node -v
```

### npm命令
```shell
npm ls pkg：查看特定package的信息

npm info pkg : 比npm ls 命令列出来的信息更详细

npm update pkg: package更新

npm search pgk：搜索
```
