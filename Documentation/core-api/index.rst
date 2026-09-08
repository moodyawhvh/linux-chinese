> 🌐 本文档由 [torvalds/linux](https://github.com/torvalds/linux) 翻译,英文原版见原项目。

======================
核心 API 文档
======================

这是一部内核核心 API 手册的开端。若能为这本手册转换（乃至撰写！）更多文档，
我们将不胜感激！

核心工具
==============

本节收录通用性文档以及“核心中的核心”文档。前者是 docbook 时代遗留下来的一大堆
kerneldoc 信息的大杂烩；真该等哪天有人腾出精力时，把它拆分整理一下。

.. toctree::
   :maxdepth: 1

   kernel-api
   workqueue
   watch_queue
   printk-basics
   printk-formats
   printk-index
   symbol-namespaces
   asm-annotations
   real-time/index
   housekeeping.rst

数据结构与底层工具
=======================================

在整个内核中被广泛使用的程序库功能。

.. toctree::
   :maxdepth: 1

   kobject
   kref
   cleanup
   assoc_array
   folio_queue
   xarray
   maple_tree
   idr
   circular-buffers
   rbtree
   generic-radix-tree
   packing
   this_cpu_ops
   timekeeping
   errseq
   wrappers/atomic_t
   wrappers/atomic_bitops
   floating-point
   union_find
   min_heap
   parser
   list

底层入口与退出
========================

.. toctree::
   :maxdepth: 1

   entry

并发原语
======================

Linux 是如何避免所有事情同时发生的。更多相关文档参见
Documentation/locking/index.rst。

.. toctree::
   :maxdepth: 1

   refcount-vs-atomic
   irq/index
   local_ops
   padata
   ../RCU/index
   wrappers/memory-barriers.rst
   SMP

底层硬件管理
=============================

缓存管理、CPU 热插拔管理等。

.. toctree::
   :maxdepth: 1

   cachetlb
   cpu_hotplug
   memory-hotplug
   genericirq
   protection-keys

内存管理
=================

介绍如何在内核中分配和使用内存。注意，Documentation/mm/index.rst 中还有
大量更为详尽的内存管理文档。

.. toctree::
   :maxdepth: 1

   memory-allocation
   unaligned-memory-access
   dma-api
   dma-api-howto
   dma-attributes
   dma-isa-lpc
   swiotlb
   mm-api
   cgroup
   genalloc
   pin_user_pages
   boot-time-mm
   gfp_mask-from-fs-io
   kho/index

内核调试接口
===============================

.. toctree::
   :maxdepth: 1

   debug-objects
   tracepoint
   debugging-via-ohci1394

其他内容
===============

放不进其他章节、或尚未归类的文档。

.. toctree::
   :maxdepth: 1

   librs
   liveupdate
   netlink
