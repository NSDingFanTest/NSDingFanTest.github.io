---
layout:     post
title:      Mac 上使用 Visual Studio Code 进行Python开发
subtitle:   Python 安装&配置 - VSC 设置
header-img: img/post-bg-tamaki.jpg
date:       2018-08-12
author:     DF
catalog:    true
tags:
    - Python
    - VS Codo
    - 配置
---

>在Visual Studio Code 的官方网站中，已经有在 VSCode 上使用 Python 的详细配置了， 
>
>链接：[Python in Visual Studio Code](https://code.visualstudio.com/docs/languages/python){:target="_blank"}


## Python 的安装 (macOS)
#### 1. 检查已安装的 Python 版本

1. 检查是否安装了python2

    ```
    $ python
    Python 2.7.15 (default, Jun 17 2018, 12:46:58) 
    [GCC 4.2.1 Compatible Apple LLVM 9.1.0 (clang-902.0.39.2)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 
    ```

2. 检查是否安装了python3

    ```
    $ python3
    Python 3.6.6 (v3.6.6:4cf1f54eb7, Jun 26 2018, 19:50:54) 
    [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.57)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 
    ```

> 如果得到以上输出，则表示对应的版本已安装。

#### 2. 使用 Homebrew 来安装 Python3

1. 安装Homebrew

    Homebrew依赖于Apple包Xcode，需要先进行安装。出现的对话框点OK即可。
    
    ```
    $ xcode-select --install
    ```
    
    安装[Homebrew](https://brew.sh/index_zh-cn)：
    
    ```
    $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```

    确认Homebrew安装正确
    
    ```
    $ brew doctor
    ```
    
2. 安装Python3

    ```
     $ brew install python3
    ```
    确认安装版本

    ```
    $ python3 --version
    Python 3.6.6
    ```


## VSCode 的配置

#### 1. 安装VSCode的Python扩展插件

<!--[](){:target="_blank"}-->
[ ![Snip20180812_10-w400](/img/Snip20180812_10.png)](https://marketplace.visualstudio.com/items?itemName=ms-python.python){:target="_blank"}
    点击图片可以前往链接，直接点击Install安装即可。或者也可以直接在VSCode的扩展栏（Shift + Command + X）中搜索Python，安装下载量最高的那个。安装完成后，就可以使用VSCode编辑python代码了，按F5即可进行debug。

#### 2. 设置调试使用的 Python 版本

Mac上自带的 Python 版本为 2.7.x，VSCode 的调试（Python:Terminal(intergrated）)默认会使用系统自带的 Python 版本。而如果我们另外安装了 Python3，希望用 Python3 进行调试的时候，就需要修改一下  VSCode 的设置了。

设置步骤:
1. 打开VSCode用户设置（Code - 首选项 - 设置）    
    
    ![Snip20180812_12-w400](/img/Snip20180812_12.png)
    
2. 输入`"python.pythonPath":"python3",`
    
    ![Snip20180812_15](/img/Snip20180812_15.png)

---
> 以下两个插件需要预先安装`pip`

<!--MarkDown 加锚点-->

#### 3. 配置yapf
    
1. yapf简介
    [yapf](https://github.com/google/yapf){:target="_blank"}是一个源代码格式化工具，可以辅助我们进行代码的格式化，安装完成后，通过快捷键 `Shift + Alt + F` 执行代码的格式化。
    
1. 安装步骤

    1. 打开命令行，输入 `pip install yapf`
    
    2. 安装成功后， 打开VSCode用户设置（Code - 首选项 - 设置）
        
        ![Snip20180812_12-w400](/img/Snip20180812_12.png)
        在`setting.json`文件中， 输入`"python.formatting.provider": "yapf",`
        
        ![Snip20180812_14-w400](/img/Snip20180812_14.png)

1. yapf使用技巧
    1. 设置 代码段禁用自动格式化
        通过 `# yapf: disable` 以及 `# yapf: enable`， 将不需要调整格式的代码段包括其中，即可在某些代码段禁用自动格式化。
        ```
        # yapf: disable
        # 这里的代码不会被自动调整格式
        # yapf: enable
        ```
        
    2. 更多yafp用法参考
        
        在终端中输入：`$ yapf -h`，查看相关帮助。

#### 4. 配置flake8

1. flake8 简介
    
    [Flake8](https://pypi.org/project/flake8/){:target="_blank"}包装了下列工具：
        
    - PyFlakes：静态检查Python代码逻辑错误的工具。
    - pep8： 静态检查PEP 8编码风格的工具。
    - Ned Batchelder’s McCabe script：静态分析Python代码复杂度的工具。
    通过flake8可以帮助我们避免以及查找错误，并规范格式。
    
2. 安装步骤：
    1. 打开命令行，输入 `pip install flake8`
    2. 打开VSCode用户设置（Code - 首选项 - 设置）
        
        ![Snip20180812_12-w400](/img/Snip20180812_12.png)
        在`setting.json`文件中， 输入`"python.linting.flake8Enabled":true,`
        
        ![Snip20180812_13-w400](/img/Snip20180812_13.png)

3. flake8 的一些配置：
    
    如果我们的使用习惯和flake8的默认设定有冲突，可以对flake8进行配置。
    在控制台中，输入 `flake8 --help`， 会显示flake8可以设置的参数。我们可以在VSCode的Setting中对flake8的这些参数进行设置。
    在用户设置中添加 `"python.linting.flake8Args": [],` 这条设置，并在其中添加需要修改的条目即可。
    ![Snip20180812_12-w400](/img/Snip20180812_12.png)
    ![](/img/15349299826024.jpg)
    
    1. 调整flake8单行代码长度的检测：
        
        ```json
        // 单行代码最大长度改为300
        
        "python.linting.flake8Args": ["--max-line-length=300"]
        ```

## VSCode 的其他设置

1. 设置代码长度
    
    ```
    // 80，120 表示分别在80和120字符处显示一条辅助线，可以进行设置和调整。
    "editor.rulers": [80,120]
    ```
    
## 参考

- [post-image: pixiv-id-6675416](https://www.pixiv.net/member.php?id=6675416){:target="_blank"}
- [vscode 编写python如何禁止 flake8 提示 line too long](https://www.cnblogs.com/tangxin-blog/p/6065017.html){:target="_blank"}
- [《Python编程：从入门到实践》](https://book.douban.com/subject/26829016/){:target="_blank"}