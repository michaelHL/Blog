---
title: Perl 5 拾零
date: 2018-01-10 11:25:35
tags:
    - Perl
categories: Memos
mathjax: true
---

Perl 5 笔记.

<!-- more -->

1. `use 5.016` 表示程序只能在 Perl 5.16 以上的版本运行，另外版本号一定要写成三位数，包括前导零
1. 整数直接量可以用 `_` 分隔以提高阅读体验，比如 `0x50_65_72_7c`
1. 字符串用点号 `.` 拼接
1. 字符串重复操作符 `x` 可以让左边的操作数重复右边的操作数进行连接
1. 语句末尾的 `;` 除了在块中末尾行，都是必须的
1. 单引号内能转义的字符仅有 `'` 及 `\`
1. 双引号内字符串的转义字符有(特殊):
   - `\e` -- Esc (ASCII 编码的转义字符)
   - `\007` -- 八进制
   - `\x7f` -- 十六进制
   - `\x{2744}` -- 十六进制的 Unicode 代码点
   - `\cC` -- Control 键的代码
   - `\l` / `\L` -- 将下个/后面所有字母转为小写
   - `\u` / `\U` -- 将下个/后面所有字母转为大写
   - `\Q` -- 把它到 `\E` 之间的非单词字符加上反斜线转义
   - `\E` -- 结束 `\L`，`\U`，`\Q` 开始的作用范围
1. 获取用户输入并去除末尾的换行符(标量 / 数组)：
   {% codeblock lang:perl %}
   chomp(my $text = <STDIN>);
   chomp(my @texts = <STDIN>);
   {% endcodeblock %}
1. 判断是否为 `undef`：`defined` 函数
1. `$#foo` 返回 `@foo` 的最大索引值
1. 数组内插到双引号串中时自动添加分隔用的空格，空格这个分隔符的变量为 `$"`
1. `each` 操作符与 Python 中的 `enumerate` 相似，会返回数组中的元素索引以及值
1. 强制指定标量上下文，比如
   {% codeblock lang:bash %}
   my @rocks = qw ! talc quartz jade obsidian !;
   print "How many rocks do you have?\n";
   print "I have ", @rocks, " rocks!\n";
   print "I have ", scalar @rocks, " rocks!\n";
   {% endcodeblock %}
1. 避免子程序名称与内置函数重名，应使用 `&`，或者打开警告开关 `use warnings`
1. 不能在 `qw` 函数中运用字符串插值
1. 如果 `print` 或者其它函数名后面接着一个左括号，务必保证这个括号是成对的
1. 赋值语句返回实际的变量作为左值
1. 在同一个正则表达式中，反向引用为 `\1`，否则使用 `$1`
1. Perl 提供两种类型的名字空间，符号表 (symbol table) 和词法作用域
   (lexical scope)
1. `$str =~ tr/x//` 仅统计 `$str` 中 `x` 的数量，而 `tr/x//d` 删除所有的 `x`
1. Windows 用 StrawBerry Perl 安装 CPAN 包时，注意将包含 StrawBerry 的 GCC
   工具链优先级提前或者删除其它可能影响编译的路径，部分包需要强制安装：`cpan -fi Tk`
1. Windows 下 `cpan` 安装包遇到网络环境差如何设置代理：
   {% codeblock %}
   % set http_proxy=http://127.0.0.1:1234
   % cpan -fi xx::xx
   {% endcodeblock %}
1. Windows 下 `Strawberry Perl` 的 `portable` 版本中， `\Strawberry\perl\vendor\lib\Portable` 会覆盖掉模块 `File::Homedir`，导致结果可能与预期不同，应改用 `File::HomeDir::Windows`
1. **更新** 上述的问题可能与 `File::HomeDir` 的版本过低有关，重新通过 `cpan` 安装貌似可以解决
1. 在标量上下文中，`each` 仅返回 `hash` 的键，或者列表的索引
1. 从源码编译 Perl：[http://www.cpan.org/src](http://www.cpan.org/src)。文章：[How to build perl from source on Linux](https://perlmaven.com/how-to-build-perl-from-source-code).
   示例：
   {% codeblock lang:bash%}
   wget http://www.cpan.org/src/5.0/perl-5.26.1.tar.gz
   tar -xzf perl-5.26.1.tar.gz
   cd perl-5.26.1
   ./Configure -des -Dprefix=$HOME/localperl -Dusethreads -Duselargefiles \
               -Duse64bitint -Duselongdouble -Dusemorebits -Duseshrplib
   make -j4
   TEST_JOBS=4 make test_harness
   make install
   {% endcodeblock %}
1. 关于 Perl 命令行参数 `-0`：
   - `-0`：相当于 `$/ = "\0"`
   - `-00`：相当于 `$/ = ""`，段落模式
   - `-0777`：相当于 `$/ = undef`，啜食模式
1. 由 [按时间排序字符串](https://stackoverflow.com/questions/17600885/sort-strings-in-perl-according-to-date)
   而来的 [施瓦茨变换(Schwartzian transform)](https://www.wikiwand.com/en/Schwartzian_transform)
1. 安装 `cpanm`：`cpan App::cpanminus`。设置下载镜像：
   {% codeblock lang:bash %}
   alias cpanm='cpanm --mirror http://mirrors.ustc.edu.cn/CPAN --mirror http://www.cpan.org'
   {% endcodeblock %}
1. $\LaTeX$ 中展示目录树。
   [参考：[(Semi-)automatic directory-tree in LaTeX](https://texblog.org/2012/08/07/semi-automatic-directory-tree-in-latex)]
   {% codeblock lang:latex tree.pl%}
   \usepackage{dirtree}
   ...
   \dirtree{
   ...
   }
   {% endcodeblock %}
   `\dirtree` 中的部分可由 [tree.pl](/src/tree.pl) 来生成
1. 如果子程序的定义在其被调用之前，调用时可略去括号；如果定义在被调用之后，则不能省略括号。（后者也为 Perl Maven 的这篇 [文章](https://perlmaven.com/subroutines-and-functions-in-perl) 所推荐）
