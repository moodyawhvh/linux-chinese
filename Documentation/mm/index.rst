> 🌐 本文档由 [torvalds/linux](https://github.com/torvalds/linux) 翻译,英文原版见原项目。

===============================
内存管理文档
===============================

本指南旨在帮助读者理解 Linux 的内存管理(MM)子系统。如果你只是想了解如何
分配内存,请参阅 :ref:`memory_allocation`;有关控制与调优方面的指南,
请参阅 :doc:`管理员指南 <../admin-guide/mm/index>`。

.. note::

  遗憾的是,本指南仍有部分内容不完整或缺失。我们欢迎贡献,但这一领域的文档
  很难写好,需要对细节投入大量精力。新贡献者应尽早联系相关的
  maintainer(维护者)。

  本指南应力求反映真实情况,这要求贡献者具备深入细致的理解。由不熟悉这些
  细节的贡献者借助 LLM(大语言模型)生成的文档,会把真正的工作转嫁给
  审阅者,因此此类贡献将被直接拒收,恕不另行说明。

.. toctree::
   :maxdepth: 1

   physical_memory
   page_tables
   process_addrs
   bootmem
   page_allocation
   vmalloc
   slab
   highmem
   page_reclaim
   swap
   swap-table
   page_cache
   shmfs
   oom

未归类的文档
============

这里收录了一组尚未整理的文档,内容涉及 Linux 内存管理(MM)子系统的内部
实现,详细程度不一:既有随笔札记,也有来自邮件列表、深入阐述数据结构与
算法的回复。这些内容最终都应妥善整合进上述结构化文档之中;若已失去存在
价值,则应予以删除。

.. toctree::
   :maxdepth: 1

   active_mm
   allocation-profiling
   arch_pgtable_helpers
   balance
   damon/index
   free_page_reporting
   hmm
   hwpoison
   hugetlbfs_reserv
   ksm
   memory-model
   memfd_preservation
   mmu_notifier
   multigen_lru
   numa
   overcommit-accounting
   page_migration
   page_frags
   page_owner
   page_table_check
   remap_file_pages
   split_page_table_lock
   transhuge
   unevictable-lru
   vmalloced-kernel-stacks
   vmemmap_dedup
   zsmalloc
