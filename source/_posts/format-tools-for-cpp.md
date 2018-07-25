---
title: Format Tools for C++
date: 2018-07-11 09:50:06
tags: [C++]
categories: Coding
---

### C++ Coding Style

不同的编程语言有不同的编码风格，变量函数的命名是用匈牙利命名法(iMyName)还是驼峰命名法(myName)还是帕斯卡命名法(MyName)还是下划线命名法(my_name)，代码注释是用`//`还是`/* */`，代码缩进是用空格还是tab键，这些都是代码风格相关的内容。大公司一般都有自己的编码规范，华为有华为的一套，微软有微软的规范，谷歌也有谷歌的风格，小公司一般就实行拿来主义，照搬谷歌微软之类的就行，毕竟规范只是形式，代码本身才是内容。当然，对于开源社区的项目或已有的项目，必须遵循现存的代码风格，比如进行Linux驱动开发就必须遵循Linux内核的代码风格。对于C++的编码风格，建议使用Google C++ style。

常用的C/C++代码格式化及检测工具有cpplint，clang-format，PC-Lint，astyle等，下面只介绍cpplint和clang-format，因为我就用这两个:smile: :grin:

### cpplint

cpplint是Google推出的用来检测C++代码是否符合Google C++ style的工具，是一个Python脚本，只能用来检查，不能自动纠正。可以运行`cpplint.py --help`查看其用法。

Ubuntu下安装cpplint.py步骤如下：

```
sudo curl -L "https://raw.githubusercontent.com/google/styleguide/gh-pages/cpplint/cpplint.py" -o /usr/bin/cpplint.py
sudo chmod a+x /usr/bin/cpplint.py
```

### clang-format

clang-format是LLVM Clang编译器中自带的工具，用来格式化C/C++/Java/JavaScript/Objective-C/Protobuf等代码。使用Vim/Eclipse/Xcode等IDE工具可以配置集成clang-format，Vim有[vim-clang-format][]插件，Eclipse有[CppStyle][]插件。使用clang-format需要指定具体的coding style，在代码根目录下执行以下命令：

`$ clang-format -dump-config -style=Google > .clang-format`

clang-format默认使用的Google style有一个选项AllowShortFunctionsOnASingleLine，默认值是All，允许很短的函数或inline函数(在class内部定义的函数)或空函数写成一行，这种style大部分人都会不习惯，所以改成Empty，只将空函数写成一行。

```
$ clang-format -dump-config -style="{BasedOnStyle: Google, AllowShortFunctionsOnASingleLine: Empty}" > .clang-format
```

### Reference

[Google C++ Style](https://google.github.io/styleguide/cppguide.html)

[cpplint](https://github.com/google/styleguide/tree/gh-pages/cpplint)

[clang-format](http://clang.llvm.org/docs/ClangFormat.html)

[vim-clang-format]: https://github.com/rhysd/vim-clang-format "vim-clang-format"

[CppStyle]: http://www.cppstyle.com/ "CppStyle"
