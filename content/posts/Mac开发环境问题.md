+++
title = 'Mac开发环境问题'
date = 2024-11-26T20:19:54+08:00
menu = ['posts']
tags = ['macos', 'develop']
+++
## Big Sur及以后版本，根目录无法创建文件夹

```
## 0.查看synthetic.conf说明
man synthetic.conf
```

```
## 1.vim 修改synthetic.conf（没有会创建）
sudo vim /etc/synthetic.conf

## 2.添加一行记录(使用 tab 进行分割，空格 换行，使用空格分割会发现重启无效)

## 以此处举例

data    /Users/admin/data

## 重启后，在根目录下已经创建好 data 软连接到 /Users/admin/data
ls -al
......
lrwxr-xr-x   1 root  wheel    17  7 21 14:45 data -> /Users/admin/data
......
```


