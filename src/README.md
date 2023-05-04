# 介绍

## 实验前阅读

- [学术诚信（什么事情能做，什么不能）](http://integrity.mit.edu/)
- 请务必仔细阅读[配置git](dev-env/install-dependency.md#配置git)一节，这关乎你最终项目能否验收通过
- 如何正确向助教提问：[提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)和[Stop-Ask-Questions-The-Stupid-Ways](https://github.com/tangx/Stop-Ask-Questions-The-Stupid-Ways/blob/master/README.md)
- 外部资源
    - [CMU 15445](https://15445.courses.cs.cmu.edu/spring2023/)
        - 卡耐基梅隆大学的数据库课程网站
        - 如果你对数据库系统感兴趣，那么强烈推荐自学这门课（然后就可以联系[宫学庆老师](mailto:xqgong@sei.ecnu)来实验室做超酷的项目啦🤩）
    - [一个简短的C++教程](https://www.thegeekstuff.com/2016/02/c-plus-plus-11/)
    - [cpp reference](https://en.cppreference.com/w/)提供了非常详细的C++标准文档
    - [GDB调试入门指南](https://zhuanlan.zhihu.com/p/74897601)
    - [GDB Cheatsheet](https://darkdust.net/files/GDB%20Cheat%20Sheet.pdf)

---
**关于网络**

实验文档托管在github page上，无法稳定魔法上网的同学可以点击右上角下载pdf版本。但由于该文档还处于实验阶段，随时可能进行更新，阅读pdf版本可能无法及时获取最新版的实验文档，所以还是推荐同学们设法稳定魔法上网。

--- 

**如何求助**

- 如果你在实验过程中遇到了困难，并打算向助教寻求帮助，请先阅读[提问的智慧](https://github.com/ryanhanwu/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)和[Stop-Ask-Questions-The-Stupid-Ways](https://github.com/tangx/Stop-Ask-Questions-The-Stupid-Ways/blob/master/README.md)这两篇文档。
- 如果你发现了实验文档的错误、不严谨或者对实验内容有疑问或建议，建议通过右上角Github Issue向我们提出建议。

## 实验方案

理解数据库系统的根本途径是从零开始实现一个完整的数据库系统。但对于我们的课程项目来说从零做一个完整系统的工作量是"超模"的，于是我们基于MySQL8.0设计了[DB20XX](https://github.com/FLAYhhh/DB20XX)教学系统。本学期，同学们需要在该系统中完成一个B+树索引。

## 实验环境

- 编程语言：C++17
- 操作系统：Ubuntu 20.04(使用其他发行版或其他版本的Ubuntu不保证文档可用，需要你自行探索如何操作)
- 编译器：GCC

## 如何查阅资料

在学习和实验的过程中, 你会遇到大量的问题. 除了参考课本内容之外, 你需要掌握如何获取其它参考资料.

但在此之前, 你需要适应查阅英文资料。

如何适应查阅英文资料? 方法是尝试并坚持查阅英文资料.

|   | 搜索引擎 | 百科 | 问答网站 |
|:--|:---------|:-----|:---------|
|推荐使用| <https://google.com> | <https://en.wikipedia.org> | <https://stackoverflow.com>         |
|不推荐使用| ~~<https://baidu.com>~~| ~~<https://baike.baidu.com>~~|~~<https://zhidao.baidu.com>~~</br>~~<https://bbs.csdb.net>~~|

一些说明：
- 一般来说，百度对英文关键词的处理能力比不上Google。
- 通常来说，英文维基百科比中文维基百科和百度百科包含更丰富的内容。 为了说明为什么要使用英文维基百科, 请你对比词条`Boyce-Codd范式`分别在[百度百科](https://baike.baidu.com/item/bcnf?fromModule=lemma_search-box)，[中文维基百科](https://zh.wikipedia.org/wiki/BC%E6%AD%A3%E8%A6%8F%E5%BD%A2%E5%BC%8F)和[英文维基百科](https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form)中的内容。
- stackoverflow是一个程序设计领域的问答网站，里面除了技术性的问题([What is ":-!!" in C code?](https://stackoverflow.com/questions/9229601/what-is-in-c/9229793#9229793))之外, 也有一些学术性([Is there a regular expression to detect a valid regular expression?](https://stackoverflow.com/questions/172303/is-there-a-regular-expression-to-detect-a-valid-regular-expression)) 和一些有趣的问题([What is the “-->” operator in C++?](https://stackoverflow.com/questions/1642028/what-is-the-operator-in-c-c))。
- 自从ChatGPT诞生以后许多问题可以直接问GPT，或许能获得比搜索引擎更好的答案，同学们也不妨使用。

[^nju-pa-intro]: 本章节选并改编自[南京大学计算机系统基础课程实验课程文档](https://nju-projectn.github.io/ics-pa-gitbook/ics2022/#%E5%AE%9E%E9%AA%8C%E5%89%8D%E9%98%85%E8%AF%BB)
