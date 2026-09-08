<div align="center">

# linux 中文翻译版

**[中文版] linux — Linux 内核源代码树:整个 Linux 操作系统的核心,负责管理硬件与系统资源,并为所有其他软件提供基础服务**

[![原项目](https://img.shields.io/badge/原项目-torvalds--linux-blue?style=flat-square&logo=github)](https://github.com/torvalds/linux)
[![中文文档](https://img.shields.io/badge/中文文档-README.zh--CN.md-orange?style=flat-square)](README.zh-CN.md)
[![GitHub Stars](https://img.shields.io/github/stars/torvalds/linux?style=flat-square&label=原项目Stars)](https://github.com/torvalds/linux/stargazers)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

---

> 这是 [torvalds/linux](https://github.com/torvalds/linux) 的中文翻译版本。
> 完整源代码请访问原项目:https://github.com/torvalds/linux

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

## 📖 项目简介

Linux 内核是任何 Linux 操作系统的心脏:它直接管理 CPU、内存、外设等硬件,调度系统资源,并为上层所有软件提供最基础的运行服务。从手机、路由器到超级计算机和云端服务器,全球无数设备都运行着这套内核。本仓库是 [torvalds/linux](https://github.com/torvalds/linux) 官方 README 的中文翻译介绍版,帮助中文读者快速理解内核的定位、获取方式、编译入口,以及按角色分类的官方文档导航。完整源代码请访问原项目,本仓库不含任何源代码,仅做文档翻译。

## ✨ 主要特性

- 🧠 **操作系统核心** — 统一管理硬件、内存与系统资源,是所有 Linux 发行版的共同基石
- 🌍 **无处不在** — 驱动着服务器、安卓设备、嵌入式系统、超算等几乎所有计算场景
- 📚 **文档体系完整** — `Documentation/` 目录覆盖开发流程、内核 API、驱动模型、安全加固等全套官方文档
- 👥 **按角色导航** — 官方 README 为新手开发者、学术研究者、安全专家、运维、维护者、硬件厂商等提供精准的文档入口
- 🛡️ **安全机制成熟** — 内置 LSM、Seccomp-BPF、自我保护等安全框架,并提供漏洞报告与 CVE 流程
- 🔧 **构建系统完善** — Kconfig/Kbuild 支持高度可定制的内核配置与编译
- 🤝 **社区活跃** — 邮件列表、IRC、Bugzilla 多渠道支持,贡献流程规范透明
- ⚖️ **GPL-2.0 开源** — 自由软件许可证,代码与文档均可自由学习、修改与分发

## 📁 文件说明

| 文件 | 说明 |
|:-----|:-----|
| README.md | 本文件(中文简介) |
| README.zh-CN.md | 详细中文文档(完整汉化) |

## 🚀 快速开始

1. **报告问题**:参考 `Documentation/admin-guide/reporting-issues.rst`
2. **获取最新内核**:访问 https://kernel.org

   ```bash
   git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
   ```

3. **编译内核**:参考 `Documentation/admin-guide/quickly-build-trimmed-linux.rst`,常见流程:

   ```bash
   make defconfig
   make -j$(nproc)
   ```

4. **了解构建依赖**:阅读 `Documentation/process/changes.rst`
5. **查阅许可证**:见源码树根目录的 `COPYING` 文件(GPL-2.0)
6. **构建/在线阅读文档**:

   ```bash
   make htmldocs
   ```

   或在线访问:https://www.kernel.org/doc/html/latest/
7. **加入社区**:邮件列表 https://lore.kernel.org/ ,IRC 频道 `#kernelnewbies`(irc.oftc.net)

完整源代码与最新版本请访问原项目:https://github.com/torvalds/linux

## 📞 联系方式

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

本项目为 [torvalds/linux](https://github.com/torvalds/linux) 的中文翻译版本,所有代码版权归原项目作者所有,遵循其原始许可证(GPL-2.0)。

**如果觉得有用,请给原项目点个 Star!** ⭐
