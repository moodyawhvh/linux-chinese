> 🌐 本文档由 [torvalds/linux](https://github.com/torvalds/linux) 翻译,英文原版见原项目。

=================================================
Linux 内核用户和管理员指南
=================================================

以下是随时间推移陆续收入内核的面向用户的文档合集。目前这些内容还谈不上有什么整体顺序或组织结构——毕竟这些材料本来就不是作为一份连贯的单一文档编写的!但愿情况能随着时间推移快速改善。

内核管理通用指南
---------------------------------------

本节开头部分包含总体性信息,包括对内核整体进行说明的 README 文件、内核参数文档等。

.. toctree::
   :maxdepth: 1

   README
   devices

   features

内核管理接口的很大一部分是 /proc 和 sysfs 这两个虚拟文件系统;这些文档描述了如何与它们交互。

.. toctree::
   :maxdepth: 1

   sysfs-rules
   sysctl/index
   cputopology
   abi

安全性相关文档:

.. toctree::
   :maxdepth: 1

   hw-vuln/index
   LSM/index
   perf-security

内核引导
------------------

.. toctree::
   :maxdepth: 1

   bootconfig
   kernel-parameters
   efi-stub
   initrd


问题的追踪与定位
--------------------------------------

这里汇集了一组文档,专门面向试图追踪问题和缺陷(bug)的用户。

.. toctree::
   :maxdepth: 1

   reporting-issues
   reporting-regressions
   quickly-build-trimmed-linux
   verify-bugs-and-bisect-regressions
   bug-hunting
   bug-bisect
   tainted-kernels
   ramoops
   dynamic-debug-howto
   init
   kdump/index
   perf/index
   pstore-blk
   clearing-warn-once
   kernel-per-CPU-kthreads
   lockup-watchdogs
   RAS/index
   sysrq


内核核心子系统
----------------------

这些文档描述了几乎在任何系统上都值得关注的核心内核管理接口。

.. toctree::
   :maxdepth: 1

   cgroup-v2
   cgroup-v1/index
   cpu-isolation
   cpu-load
   mm/index
   module-signing
   namespaces/index
   numastat
   pm/index
   syscall-user-dispatch

对非本地(non-native)二进制格式的支持。注意,其中一些文档已经……有些年头了……

.. toctree::
   :maxdepth: 1

   binfmt-misc
   java
   mono


块层与文件系统管理
-----------------------------------------

.. toctree::
   :maxdepth: 1

   bcache
   binderfs
   blockdev/index
   cifs/index
   device-mapper/index
   ext4
   filesystem-monitoring
   nfs/index
   iostats
   jfs
   md
   ufs
   xfs

设备专用指南
----------------------

介绍如何在你的 Linux 系统中配置硬件。

.. toctree::
   :maxdepth: 1

   acpi/index
   aoe/index
   auxdisplay/index
   braille-console
   btmrvl
   dell_rbu
   edid
   gpio/index
   hw_random
   laptops/index
   lcd-panel-cgram
   media/index
   nvme-multipath
   parport
   pnp
   rapidio
   rtc
   serial-console
   svga
   thermal/index
   thunderbolt
   vga-softcursor
   video-output

工作负载分析
-----------------

本节刚刚起步,收录的信息面向在安全关键型(safety-critical)应用场景中分析 Linux 内核的应用开发者和系统集成者。这里将汇集有助于分析内核与应用程序之间交互的文档,以及关键内核子系统的行为预期说明。

.. toctree::
   :maxdepth: 1

   workload-tracing

其余内容
---------------

一些难以归类、总体上已过时的文档。

.. toctree::
   :maxdepth: 1

   ldm
   unicode
