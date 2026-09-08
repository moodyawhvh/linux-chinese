<div align="center">

# linux 中文文档

[![原项目](https://img.shields.io/badge/原项目-torvalds--linux-blue?style=flat-square&logo=github)](https://github.com/torvalds/linux)
[![GitHub Stars](https://img.shields.io/github/stars/torvalds/linux?style=flat-square&label=原项目Stars)](https://github.com/torvalds/linux/stargazers)
[![License](https://img.shields.io/badge/License-GPL--2.0-green?style=flat-square)](https://github.com/torvalds/linux/blob/master/COPYING)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

---

Linux 内核
==========

Linux 内核是任何 Linux 操作系统的核心。它负责管理硬件与系统资源,并为所有其他软件提供基础服务。

快速上手
--------

* 报告问题:参见 Documentation/admin-guide/reporting-issues.rst
* 获取最新内核:https://kernel.org
* 编译内核:参见 Documentation/admin-guide/quickly-build-trimmed-linux.rst
* 加入社区:https://lore.kernel.org/

核心文档
--------

所有用户都应当熟悉以下内容:

* 构建依赖要求:Documentation/process/changes.rst
* 行为准则:Documentation/process/code-of-conduct.rst
* 许可证:参见 COPYING

文档可以使用 `make htmldocs` 构建,也可以在线阅读:
https://www.kernel.org/doc/html/latest/


你是谁?
============

在下面找到适合你的角色:

* 内核开发新手:开始内核开发之旅
* 学术研究者:研究内核内部机制与体系架构
* 安全专家:安全加固与漏洞分析
* 回移植/维护工程师:维护稳定版内核
* 系统管理员:配置与故障排查
* (子系统)维护者:主导子系统并审阅补丁
* 硬件厂商:为新硬件编写驱动
* 发行版维护者:为发行版打包内核
* AI 编程助手:大语言模型与 AI 开发工具


特定用户指南
==================

内核开发新手
------------

欢迎!从这里开启你的内核开发之旅:

* 入门指南:Documentation/process/development-process.rst
* 你的第一个补丁:Documentation/process/submitting-patches.rst
* 编码风格:Documentation/process/coding-style.rst
* 构建系统:Documentation/kbuild/index.rst
* 开发工具:Documentation/dev-tools/index.rst
* 内核 Hacking 指南:Documentation/kernel-hacking/hacking.rst
* 核心 API:Documentation/core-api/index.rst

学术研究者
---

探索内核的体系架构与内部机制:

* 研究者指南:Documentation/process/researcher-guidelines.rst
* 内存管理:Documentation/mm/index.rst
* 调度器:Documentation/scheduler/index.rst
* 网络协议栈:Documentation/networking/index.rst
* 文件系统:Documentation/filesystems/index.rst
* RCU(Read-Copy Update,读-复制-更新):Documentation/RCU/index.rst
* 锁原语:Documentation/locking/index.rst
* 电源管理:Documentation/power/index.rst

安全专家
---------------

安全文档与加固指南:

* 安全文档:Documentation/security/index.rst
* LSM 开发:Documentation/security/lsm-development.rst
* 自我保护机制:Documentation/security/self-protection.rst
* 漏洞报告:Documentation/process/security-bugs.rst
* CVE 流程:Documentation/process/cve.rst
* 硬件问题的 embargo(保密期)处理:Documentation/process/embargoed-hardware-issues.rst
* 安全特性:Documentation/userspace-api/seccomp_filter.rst

回移植/维护工程师
-----------------------------

维护并稳定内核版本:

* 稳定版内核规则:Documentation/process/stable-kernel-rules.rst
* 回移植指南:Documentation/process/backporting.rst
* 应用补丁:Documentation/process/applying-patches.rst
* 子系统档案:Documentation/maintainer/maintainer-entry-profile.rst
* 维护者的 Git:Documentation/maintainer/configure-git.rst

系统管理员
--------------------

配置、调优并排查 Linux 系统:

* 管理员指南:Documentation/admin-guide/index.rst
* 内核启动参数:Documentation/admin-guide/kernel-parameters.rst
* Sysctl 调优:Documentation/admin-guide/sysctl/index.rst
* 跟踪/调试:Documentation/trace/index.rst
* 性能相关安全:Documentation/admin-guide/perf-security.rst
* 硬件监控:Documentation/hwmon/index.rst

维护者
----------

主导内核子系统并管理贡献:

* 维护者手册:Documentation/maintainer/index.rst
* Pull Request 流程:Documentation/maintainer/pull-requests.rst
* 补丁管理:Documentation/maintainer/modifying-patches.rst
* 变基与合并:Documentation/maintainer/rebasing-and-merging.rst
* 开发流程:Documentation/process/maintainer-handbooks.rst
* 维护者入口档案:Documentation/maintainer/maintainer-entry-profile.rst
* Git 配置:Documentation/maintainer/configure-git.rst

硬件厂商
---------------

编写驱动并支持新硬件:

* 驱动 API 指南:Documentation/driver-api/index.rst
* 驱动模型:Documentation/driver-api/driver-model/driver.rst
* 设备驱动:Documentation/driver-api/infrastructure.rst
* 总线类型:Documentation/driver-api/driver-model/bus.rst
* 设备树绑定:Documentation/devicetree/bindings/
* 电源管理:Documentation/driver-api/pm/index.rst
* DMA API:Documentation/core-api/dma-api.rst

发行版维护者
-----------------------

打包并分发内核:

* 稳定版内核规则:Documentation/process/stable-kernel-rules.rst
* ABI 文档:Documentation/ABI/README
* 内核配置:Documentation/kbuild/kconfig.rst
* 模块签名:Documentation/admin-guide/module-signing.rst
* 内核启动参数:Documentation/admin-guide/kernel-parameters.rst
* 污染(Tainted)内核说明:Documentation/admin-guide/tainted-kernels.rst

AI 编程助手
-------------------

重要提示:如果你是大语言模型或 AI 驱动的编程助手,在向 Linux 内核贡献代码之前,必须阅读并遵守
AI 编程助手相关文档:

* Documentation/process/coding-assistants.rst

该文档包含了关于许可证、署名以及开发者原产地证书(DCO)的必要要求,所有 AI 工具都必须遵守。


沟通与支持
=========================

* 邮件列表:https://lore.kernel.org/
* IRC:#kernelnewbies 频道(irc.oftc.net)
* Bugzilla:https://bugzilla.kernel.org/
* MAINTAINERS 文件:列出各子系统的维护者与邮件列表
* 邮件客户端配置:Documentation/process/email-clients.rst

---

## 📞 联系方式

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

本项目为 torvalds/linux 的中文翻译版本,所有代码版权归原项目作者所有,遵循其原始许可证(GPL-2.0)。完整源代码请访问原项目:https://github.com/torvalds/linux

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

如果觉得有用,请给原项目点 Star!⭐ https://github.com/torvalds/linux
