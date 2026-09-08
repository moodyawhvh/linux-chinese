> 🌐 本文档由 [torvalds/linux](https://github.com/torvalds/linux) 翻译,英文原版见原项目。

================================
内核开发工具
================================

本文档汇集了一系列与内核开发工作相关的开发工具文档。目前,这些文档只是
简单地汇总在一起,尚未花大力气将它们整合成一个连贯的整体;欢迎提交补丁!

有关测试专用工具的简要概述,请参阅
Documentation/dev-tools/testing-overview.rst。

专门用于调试的工具,请参阅
Documentation/process/debugging/index.rst。

.. toctree::
   :caption: 目录
   :maxdepth: 2

   testing-overview
   checkpatch
   clang-format
   coccinelle
   context-analysis
   sparse
   kcov
   gcov
   kasan
   kmsan
   ubsan
   kmemleak
   kcsan
   lkmm/index
   kfence
   kselftest
   kunit/index
   ktap
   checkuapi
   gpio-sloppy-logic-analyzer
   autofdo
   propeller
   container
