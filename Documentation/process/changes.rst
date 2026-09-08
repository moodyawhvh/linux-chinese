> 🌐 本文档由 [torvalds/linux](https://github.com/torvalds/linux) 翻译,英文原版见原项目。

.. _changes:

编译内核的最低软件需求
++++++++++++++++++++++++++++++++++++++++++

简介
====

本文档旨在列出运行当前内核版本所需的软件最低版本要求。

本文档最初基于我为 2.0.x 内核编写的 "Changes" 文件,因此要感谢与那份文件相同的一批贡献者(Jared Mauch、Axel Boldt、Alessandro Sigala,以及互联网上无数其他用户)。

当前最低需求
****************************

在认定自己遇到了 bug 之前,请先把软件升级到**至少**下表所列的版本!如果不确定当前运行的是什么版本,表中给出的建议命令会告诉你。要列出系统上这些程序及其版本,可执行 ./scripts/ver_linux

再次提醒:这份清单假定你的系统已经能在功能层面正常运行某个 Linux 内核。另外,并非所有系统都需要所有工具;显然,举例来说,如果你没有任何 PC Card 硬件,多半就不必关心 pcmciautils。

====================== ===============  ========================================
         程序             最低版本                   检查版本的命令
====================== ===============  ========================================
bash                   4.2              bash --version
bc                     1.06.95          bc --version
bindgen (可选)         0.71.1           bindgen --version
binutils               2.30             ld -v
bison                  2.0              bison --version
btrfs-progs            0.18             btrfs --version
Clang/LLVM (可选)      17.0.1           clang --version
e2fsprogs              1.41.4           e2fsck -V
flex                   2.5.35           flex --version
gdb                    7.2              gdb --version
GNU awk (可选)         5.1.0            gawk --version
GNU C                  8.1              gcc --version
GNU make               4.0              make --version
GNU tar                1.28             tar --version
GRUB                   0.93             grub --version || grub-install --version
gtags (可选)           6.6.5            gtags --version
iptables               1.4.2            iptables -V
jfsutils               1.1.3            fsck.jfs -V
kmod                   13               kmod -V
mcelog                 0.6              mcelog --version
mkimage (可选)         2017.01          mkimage --version
nfs-utils              1.0.5            showmount --version
openssl & libcrypto    1.0.0            openssl version
pahole                 1.26             pahole --version
pcmciautils            004              pccardctl -V
PPP                    2.4.0            pppd --version
procps                 3.2.0            ps --version
Python                 3.9.x            python3 --version
quota-tools            3.09             quota -V
Rust (可选)            1.85.0           rustc --version
Sphinx\ [#f1]_         3.4.3            sphinx-build --version
squashfs-tools         4.0              mksquashfs -version
udev                   081              udevadm --version
util-linux             2.10o            mount --version
xfsprogs               2.6.0            xfs_db -V
====================== ===============  ========================================

.. [#f1] 只有在构建内核文档时才需要 Sphinx

内核编译
******************

GCC
---

gcc 的版本要求可能因计算机 CPU 类型的不同而有所差异。

Clang/LLVM (可选)
---------------------

内核构建支持 `releases.llvm.org <https://releases.llvm.org>`_ 上发布的最新正式版 clang 及 LLVM 工具链。更旧的版本不保证可用,而且内核中那些为支持旧版本而引入的变通代码可能会被移除。更多内容请参见 :ref:`使用 Clang/LLVM 构建 Linux <kbuild_llvm>`。

Rust (可选)
---------------

需要较新版本的 Rust 编译器。

关于如何满足 Rust 支持的构建需求,请参阅 Documentation/rust/quick-start.rst。特别是,``Makefile`` 中的 ``rustavailable`` 目标可用于排查 Rust 工具链为何未被检测到。

bindgen (可选)
------------------

``bindgen`` 用于为内核的 C 语言一侧生成 Rust 绑定。它依赖 ``libclang``。

Make
----

构建内核需要 GNU make 4.0 或更高版本。

Bash
----

内核构建过程会用到一些 bash 脚本,因此需要 Bash 4.2 或更新版本。

Binutils
--------

构建内核需要 Binutils 2.30 或更新版本。

pkg-config
----------

从 4.18 开始,构建系统需要 pkg-config 来检查已安装的 kconfig 工具,并确定 'make {g,x}config' 所使用的标志设置。此前 pkg-config 虽然已经在使用,但既未经验证也没有文档说明。

Flex
----

从 Linux 4.16 起,构建系统会在构建过程中生成词法分析器(lexical analyzer),这要求 flex 2.5.35 或更高版本。

Bison
-----

从 Linux 4.16 起,构建系统会在构建过程中生成语法分析器(parser),这要求 bison 2.0 或更高版本。

pahole
------

从 Linux 5.2 起,如果选用了 CONFIG_DEBUG_INFO_BTF,构建系统会从 vmlinux 的 DWARF 信息生成 BTF(BPF Type Format),稍后内核模块也会同样处理。这要求 pahole v1.22 或更高版本。

pahole 可从发行版的 'dwarves' 或 'pahole' 软件包中获得,也可以从 https://fedorapeople.org/~acme/dwarves/ 获取。

Perl
----

构建内核需要 perl 5 以及以下模块:``Getopt::Long``、``Getopt::Std``、``File::Basename`` 和 ``File::Find``。

Python
------

有若干配置选项依赖 Python:例如 arm/arm64 的默认配置、CONFIG_LTO_CLANG、部分 DRM 可选配置、kernel-doc 工具以及文档构建(Sphinx)等。

BC
--

构建 3.10 及更高版本的内核需要 bc。

OpenSSL
-------

模块签名与外部证书处理使用 OpenSSL 程序和加密库来完成密钥创建与签名生成。

如果启用了模块签名,构建 3.7 及更高版本的内核需要 openssl;构建 4.3 及更高版本的内核还需要 openssl 的开发包。

Tar
---

如果想启用通过 sysfs 访问内核头文件的功能(CONFIG_IKHEADERS),则需要 GNU tar。

gtags / GNU GLOBAL (可选)
-----------------------------

内核构建需要 GNU GLOBAL 6.6.5 或更高版本,才能通过 ``make gtags`` 生成标签文件,原因在于它用到了 gtags 的 ``-C (--directory)`` 标志。

mkimage
-------

构建 Flat Image Tree(FIT,常用于 ARM 平台)时需要该工具。它可通过 ``u-boot-tools`` 软件包安装,也可以从 U-Boot 源码构建。具体说明见 https://docs.u-boot.org/en/latest/build/tools.html#building-tools-for-linux

GNU AWK
-------

如果希望内核构建为内建模块生成地址范围数据(CONFIG_BUILTIN_MODULE_RANGES),则需要 GNU AWK。

系统工具
****************

架构层面的变化
---------------------

DevFS 已被弃用,改用 udev(https://www.kernel.org/pub/linux/utils/kernel/hotplug/)。

32 位 UID 支持已经就绪,祝愉快!

内核函数的文档正在转向内联文档形式:以特殊格式的注释写在源码中函数定义附近。这些注释可以与 Documentation/ 目录中的 ReST 文件结合,生成更丰富的文档,并可进一步转换为 PostScript、HTML、LaTeX、ePUB 和 PDF 文件。要把 ReST 格式转换为你需要的格式,需要用到 Sphinx。

Util-linux
----------

新版本的 util-linux 为 ``fdisk`` 提供了对更大磁盘的支持,为 mount 增加了新选项,能识别更多受支持的分区类型,还有诸如此类的其他改进。你多半会想要升级它。

Ksymoops
--------

如果最糟糕的情况发生,内核出现了 oops,你可能需要 ksymoops 工具来解码,但大多数情况下并不需要。通常更推荐启用 ``CONFIG_KALLSYMS`` 来构建内核,这样它产生的转储信息本身就可读(输出效果也比 ksymoops 更好)。如果由于某种原因你的内核构建时没有启用 ``CONFIG_KALLSYMS``,而你又无法重新编译并在该选项下复现 Oops,那么仍可以用 ksymoops 来解码这个 Oops。

Mkinitrd
--------

``/lib/modules`` 文件树布局的这些变化也要求升级 mkinitrd。

E2fsprogs
---------

最新版本的 ``e2fsprogs`` 修复了 fsck 和 debugfs 中的若干缺陷。显然,升级是明智之举。

JFSutils
--------

``jfsutils`` 软件包包含该文件系统所需的工具,提供以下实用程序:

- ``fsck.jfs`` - 回放事务日志,检查并修复 JFS 格式的分区。

- ``mkfs.jfs`` - 创建 JFS 格式的分区。

- 该软件包中还提供其他文件系统工具。

Xfsprogs
--------

最新版本的 ``xfsprogs`` 包含 ``mkfs.xfs``、``xfs_db`` 和 ``xfs_repair`` 等用于 XFS 文件系统的工具。它与体系架构无关,2.0.0 及之后的任何版本都应能与这一版本的 XFS 内核代码正常配合(鉴于若干重要改进,推荐 2.6.0 或更高版本)。

PCMCIAutils
-----------

PCMCIAutils 取代了 ``pcmcia-cs``。它在系统启动时正确设置 PCMCIA 插槽,并且在内核采用模块化构建、使用热插拔子系统的情况下,为 16 位 PCMCIA 设备加载相应的模块。

Quota-tools
-----------

要使用较新的版本 2 配额格式,需要支持 32 位 uid 和 gid。Quota-tools 3.07 及更高版本具备该支持。请使用上表推荐的版本或更新版本。

Intel IA32 微码
--------------------

内核新增了一个驱动,允许更新 Intel IA32 微码,它以普通(misc)字符设备的形式提供访问。如果你没有使用 udev,可能需要先以 root 身份执行::

  mkdir /dev/cpu
  mknod /dev/cpu/microcode c 10 184
  chmod 0644 /dev/cpu/microcode

之后才能使用该功能。你可能还需要获取配套的用户态工具 microcode_ctl。

udev
----

``udev`` 是一个用户态程序,用于动态填充 ``/dev``,只为实际存在的设备创建条目。``udev`` 取代了 devfs 的基本功能,同时支持设备的持久化命名。

FUSE
----

需要 libfuse 2.4.0 或更高版本。绝对下限是 2.3.0,但挂载选项 ``direct_io`` 和 ``kernel_cache`` 将无法工作。

网络
**********

通用变化
---------------

如果你有高级的网络配置需求,大概应该考虑使用 ip-route2 中的网络工具。

包过滤 / NAT
-------------------

包过滤与 NAT 代码使用的工具与之前的 2.4.x 内核系列相同(iptables)。它仍然包含向后兼容模块,以支持 2.2.x 风格的 ipchains 和 2.0.x 风格的 ipfwadm。

PPP
---

PPP 驱动经过了重构,以支持多链路(multilink),并使其能够在多种媒体层上运行。如果你使用 PPP,请把 pppd 升级到至少 2.4.0。

如果你没有使用 udev,则必须存在设备文件 /dev/ppp,可以 root 身份通过如下命令创建::

  mknod /dev/ppp c 108 0

> 注:篇幅所限仅译核心章节,完整内容见原项目。
