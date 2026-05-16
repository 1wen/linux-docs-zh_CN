.. include:: ../../disclaimer-zh_CN.rst

:Original: Documentation/dev-tools/kunit/index.rst

================================
KUnit - Linux 内核单元测试
================================

.. toctree::
	:maxdepth: 2
	:caption: 目录：

	start
	architecture
	run_wrapper
	run_manual
	usage
	api/index
	style
	faq
	running_tips

本节介绍内核单元测试框架。

介绍
====

KUnit（Kernel unit testing framework，内核单元测试框架）为 Linux
内核内部的单元测试提供了一套通用框架。使用 KUnit，你可以把一组测试用例
组织成测试套件。测试既可以以内建方式在内核启动时运行，也可以作为模块加
载运行。KUnit 会自动在内核日志中标记并报告失败的测试用例。测试结果会以
:doc:`KTAP（Kernel - Test Anything Protocol）格式 </dev-tools/ktap>`
呈现。它的设计思路受到了 JUnit、Python 的 unittest.mock，以及
GoogleTest/GoogleMock（C++ 单元测试框架）的启发。

KUnit 测试是内核的一部分，使用 C 语言编写，用来测试内核实现中的某些部件
（例如一个 C 函数）。不计编译时间，从触发到完成，KUnit 往往能在不到
10 秒内跑完大约 100 个测试。KUnit 几乎可以测试任何内核组件，例如：文件
系统、系统调用、内存管理、设备驱动等。

KUnit 采用白盒测试方式。测试代码可以访问系统内部功能，因此它运行在内核空
间内，不受限于仅能触达用户空间暴露出来的接口。

此外，KUnit 还提供了 `kunit_tool` 脚本
（``tools/testing/kunit/kunit.py``），它可以配置 Linux 内核、在 QEMU 或
UML（:doc:`User Mode Linux </virt/uml/user_mode_linux_howto_v2>`）环境下运
行 KUnit 测试、解析测试结果，并以更友好的方式展示出来。

特性
----

- 提供用于编写单元测试的统一框架。
- 可以在任意内核体系结构上运行测试。
- 单个测试通常可以在毫秒级完成。

前提条件
--------

- 任意兼容 Linux 内核的硬件。
- 被测内核版本需要为 Linux 5.5 或更高版本。

单元测试
========

单元测试是在隔离环境中测试一段独立代码。它是测试中粒度最细的一种形式，允
许尽可能覆盖被测代码中的所有路径。当被测代码足够小，并且没有超出测试控制
范围的外部依赖（例如真实硬件）时，这种方式尤其有效。

编写单元测试
------------

编写高质量单元测试时，一个简单但非常有力的模式是：Arrange-Act-Assert。
它非常适合组织测试用例，并明确了操作顺序。

- Arrange（准备输入与目标）：在测试开始时，准备让目标函数能够工作的数据，
  例如初始化一个状态或对象。
- Act（执行目标行为）：调用被测函数或代码路径。
- Assert（断言期望结果）：验证返回值或最终状态是否符合预期。

单元测试的优点
----------------

- 从长期看可以提高测试效率和开发效率。
- 能在早期发现缺陷，因此与验收测试阶段再修复相比，成本更低。
- 有助于提升代码质量。
- 能促进开发者编写更易测试的代码。

另请参见 :ref:`kinds-of-tests`。

如何使用？
==========

你可以在 `Documentation/dev-tools/kunit/start.rst` 中找到有关编写和运行
KUnit 测试的逐步指南。

另外，也可以继续阅读 KUnit 文档的其余部分，或者直接试用
`tools/testing/kunit/kunit.py`，以及 `lib/kunit/kunit-example-test.c`
中的示例测试。

祝测试顺利！
