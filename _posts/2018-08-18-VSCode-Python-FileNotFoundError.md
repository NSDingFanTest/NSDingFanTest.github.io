---
layout:     post
title:      VSCode 中使用 Python 异常报错 FileNotFoundError 的解决办法
subtitle:   通过配置cwd参数，解决在VSCode中使用Python读取同一子目录中的txt文件时异常报错 FileNotFoundError 的问题
header-img: post-bg-sunrise-sky.jpg
date:       2018-08-18
author:     DF
catalog: true
tags:
    - Python
    - Python - FileNotFoundError
    - cwd
    - VSCode
---

## 问题：

在 VSCode 中使用 Python，通过建立一个 Python 程序读取同一子目录中的 txt 文件，并打印其内容。

#### 预期结果
程序读取到处于同一个子目录中的 content.txt 文件，并打印其内容。

```
stay hungry
stay foolish
```

#### 实际结果

debug 出现错误 ：
```
FileNotFoundError: [Errno 2] No such file or directory: 'content.txt'
```

![vscode_python_filenotefound_1-c650](/img/vscode_python_filenotefound_1.png){:target="_blank"}



## 错误分析

VSCode 在 debug 时，使用的路径不是当前 Python 文件所在目录的路径，而是默认固定路径为项目文件夹的目录。所以在 debug 时，程序会从固定的父文件夹路径中寻找这个 txt 文件，而不是在当前这个 Python 程序所在的文件夹路径中寻找。

为了验证这个想法，我给这个 txt 文件指定`一个绝对路径`或者`添加子目录的路径`时，程序可以正常读取。所以解决问题的关键，就是在 debug 时，让 debug 的路径指向当前程序所在的子目录。

![vscode_python_filenotefound_2-c650](/img/vscode_python_filenotefound_2.png){:target="_blank"}

## 解决方案

初步思路是通过在 VSCode 的 Setting 中或者 debug 的配置文件中进行配置，来解决这个问题。通过 Google 后，我在 StackOverflow 上找到了具体解决方案。

参考链接：
- [Stack Overflow - VSCode — how to set working directory for debug](https://stackoverflow.com/questions/43801142/vscode-working-directory-when-debugging-python)
- [Stack Overflow - vscode working directory when debugging python](https://stackoverflow.com/questions/43801142/vscode-working-directory-when-debugging-python)

#### 解决方式

点击VSCode侧边的调试按钮，在出现的侧栏顶端找到设置按钮（下图中齿轮），点击打开 launch.json 文件，在文件中找到调试所用的方式，这里是 `Python: Terminal (integrated)` ，添加 cwd 添加配置 `"cwd": ""` 即可。

![vscode_python_filenotefound_3-c650](/img/vscode_python_filenotefound_3.png)



再次debug程序，问题解决。

![vscode_python_filenotefound_4-c650](/img/vscode_python_filenotefound_4.png)

## 补充

在打开一个新的项目文件夹时, 就需要重新配置 launch.json 中的 cwd 参数。