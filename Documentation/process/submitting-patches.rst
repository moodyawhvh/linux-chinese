> 🌐 本文档由 [torvalds/linux](https://github.com/torvalds/linux) 翻译,英文原版见原项目。

.. _submittingpatches:

提交补丁:让你的代码进入内核的必读指南
============================================================================

对于希望向 Linux 内核提交改动的人或公司来说,如果不熟悉“这套体系”,整个流程有时会令人生畏。本文汇集了一系列建议,可以大大提高你的改动被接受的机会。

本文以相对简练的形式给出了大量建议。有关内核开发流程运作方式的详细信息,请参阅 Documentation/process/development-process.rst。另外,请阅读 Documentation/process/submit-checklist.rst,其中列出了提交代码前需要检查的事项。对于设备树绑定(device tree binding)补丁,请阅读 Documentation/devicetree/bindings/submitting-patches.rst。

本文档假定你使用 ``git`` 来准备补丁。如果你对 ``git`` 还不熟悉,强烈建议先学会使用它——无论作为内核开发者还是在其他方面,它都会让你的日子好过得多。

一些子系统和维护者树(maintainer tree)对其工作流程与要求有额外的说明,参见 Documentation/process/maintainer-handbooks.rst。

获取最新的源码树
----------------------------

如果手头没有包含最新内核源码的仓库,可以用 ``git`` 获取一个。你应该从主线(mainline)仓库开始,获取方式如下::

  git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git

不过请注意,你可能并不想直接基于主线树做开发。大多数子系统维护者都运营自己的代码树,并希望看到针对这些树准备的补丁。要找到对应代码树,可以查看 MAINTAINERS 文件中该子系统的 **T:** 条目;如果其中没有列出,直接询问维护者即可。

.. _describe_changes:

描述你的改动
---------------------

描述你的问题。无论你的补丁是一行代码的 bug 修复,还是 5000 行的新功能,背后都必须有一个促使你做这项工作的问题。要让审查者相信确实存在一个值得修复的问题,并且值得他们继续读第一段之后的内容。

描述对用户可见的影响。直接的崩溃(crash)和死机(lockup)非常有说服力,但并非所有 bug 都这么明显。即使问题是在代码审查中发现的,也要描述你认为它可能对用户产生的影响。请记住,大多数 Linux 安装运行的是来自二级稳定树或厂商/产品专用树的内核,这些树只从上游挑选特定补丁,因此请包含任何有助于你的改动向下游传播的信息:触发条件、dmesg 摘录、崩溃描述、性能回退、延迟尖峰、死机情况等。

量化优化及其权衡。如果你声称在性能、内存消耗、栈占用或二进制体积方面有改进,请给出支撑这些说法的数字。同时也要描述那些不那么显眼的代价。优化通常不是免费的,而是在 CPU、内存与可读性之间做权衡;对启发式算法而言,则是在不同工作负载之间做权衡。请描述你的优化预期的负面效应,让审查者能够权衡成本与收益。

在问题确立之后,用技术细节描述你实际采取的解决办法。用平实的语言描述改动非常重要,这样审查者才能核实代码的行为确实符合你的意图。

如果你撰写补丁描述时采用能够直接作为“commit log”进入 Linux 源码管理系统 ``git`` 的形式,维护者会感谢你。参见 :ref:`the_canonical_patch_format`。

每个补丁只解决一个问题。如果你的描述开始变长,这很可能是在提示你需要把补丁拆开。参见 :ref:`split_changes`。

提交或重新提交补丁(或补丁系列)时,必须附上完整的补丁描述及其理由。不要只说“这是第 N 版补丁(系列)”。不要指望子系统维护者会去翻早期版本或所引用的 URL 找出补丁描述,再把它填进补丁。也就是说,补丁(系列)及其描述应当是自包含的。这对维护者和审查者都有好处——有些审查者可能根本没有收到过早期版本的补丁。

用祈使语气描述你的改动,例如 “make xyzzy do frotz”,而不是 “[This patch] makes xyzzy do frotz” 或 “[I] changed xyzzy to do frotz”,就好像你在命令代码库改变它的行为一样。

如果要引用某个特定的 commit,不要只给出该 commit 的 SHA-1 ID,还请附上它的单行摘要(oneline summary),让审查者更容易了解它讲的是什么。例如::

	Commit e21d2170f36602ae2708 ("video: remove unnecessary
	platform_set_drvdata()") removed the unnecessary
	platform_set_drvdata(), but left the variable "dev" unused,
	delete it.

另外,请务必至少使用 SHA-1 ID 的前 12 个字符。内核仓库中的对象数量极其庞大,较短的 ID 完全可能撞车。请记住,即使你的 6 字符 ID 现在没有冲突,五年之后情况也可能改变。

如果网络上能找到与该改动相关的讨论或其他背景资料,请添加指向它们的 'Link:' 标签。如果补丁源自此前的邮件列表讨论或网上已有的文档,请给出指向。

链接邮件列表存档时,优先使用 lore.kernel.org 邮件存档服务。构造链接 URL 时,取邮件 ``Message-ID`` 头的内容并去掉两侧尖括号。例如::

    Link: https://lore.kernel.org/30th.anniversary.repost@klaava.Helsinki.FI

请检查链接,确保它确实有效并指向相关邮件。

不过,请尽量让你的说明在不借助外部资料的情况下也能读懂。除了给出邮件列表存档或 bug 的 URL 之外,还应总结促成该补丁(以提交版本为准)的相关讨论要点。

如果你的补丁修复了某个 bug,请使用 'Closes:' 标签,并附上指向邮件列表存档或公开 bug 跟踪器中该报告的 URL。例如::

	Closes: https://example.com/issues/1234

一些 bug 跟踪器能够在带有此类标签的 commit 被应用时自动关闭对应 issue。一些监控邮件列表的机器人也会跟踪此类标签并采取相应动作。禁止使用私有 bug 跟踪器和无效 URL。

如果你的补丁修复了某个特定 commit 中的 bug(例如你通过 ``git bisect`` 发现了问题),请使用 'Fixes:' 标签,附上 SHA-1 ID 至少前 12 个字符以及单行摘要。不要把标签拆成多行——为了简化解析脚本,标签不受“按 75 列换行”规则的约束。例如::

	Fixes: 54a4f0239f2e ("KVM: MMU: make kvm_mmu_zap_page() return the number of pages it actually freed")

下面的 ``git config`` 配置可以添加一个 pretty 格式,用于在 ``git log`` 或 ``git show`` 命令中按上述样式输出::

	[core]
		abbrev = 12
	[pretty]
		fixes = Fixes: %h (\"%s\")

调用示例::

	$ git log -1 --pretty=fixes 54a4f0239f2e
	Fixes: 54a4f0239f2e ("KVM: MMU: make kvm_mmu_zap_page() return the number of pages it actually freed")

.. _split_changes:

拆分你的改动
---------------------

把每一个**逻辑改动**拆成单独的补丁。

例如,如果你的改动对同一个驱动既包含 bug 修复又包含性能增强,就把这些改动拆成两个或更多补丁。如果你的改动既包含一次 API 更新,又包含一个使用该新 API 的新驱动,就把它们拆成两个补丁。

反过来,如果你对大量文件做的是同一个改动,就把这些改动合并为一个补丁。这样,单一逻辑改动就完整地包含在单个补丁之内。

要记住的重点是:每个补丁所做的改动都应当易于理解、能够被审查者验证。每个补丁都应当能凭自身理由站得住脚。

如果某个补丁必须依赖另一个补丁,改动才算完整,这没有问题。只需在补丁描述中注明**“本补丁依赖于补丁 X”**。

把改动拆分为一系列补丁时,要格外注意确保系列中的每个补丁应用之后,内核都能正常构建和运行。使用 ``git bisect`` 定位问题的开发者可能在任意位置从你的补丁系列中间切开;如果你在中途引入了 bug,他们是不会感谢你的。

如果你无法把补丁集精简成更少的补丁,那么一次只投递 15 个左右,然后等待审查与合入。



对改动做风格检查
------------------------

检查你的补丁是否存在基本的风格违规,详情见 Documentation/process/coding-style.rst。不做这一步只会浪费审查者的时间,并让你的补丁被拒收——很可能连看都没被看。

一个重要的例外是:把代码从一个文件移动到另一个文件时,移动代码的那个补丁不应同时对被移动的代码做任何修改。这样能清晰区分“移动代码”与“你的改动”,极大地方便对实际差异的审查,也让工具能更好地跟踪代码本身的历史。

提交之前,请用补丁风格检查器(scripts/checkpatch.pl)检查你的补丁。不过请注意,风格检查器只能当作指南,不能取代人的判断。如果保留某处违规反而让代码更好,那通常最好的选择就是维持原样。

检查器按三个级别报告:
 - ERROR:极可能有问题的地方
 - WARNING:需要仔细审查的地方
 - CHECK:需要动脑斟酌的地方

你应当能够为补丁中残留的每一处违规给出正当理由。


为你的补丁选择收件人
------------------------------------

对于任何触及他人所维护代码的补丁,都应始终抄送相应的子系统维护者(maintainer)和邮件列表;查阅 MAINTAINERS 文件和源码修订历史,弄清这些维护者是谁。scripts/get_maintainer.pl 脚本在这一步非常有用(把补丁涉及的路径作为参数传给 scripts/get_maintainer.pl)。如果你找不到所涉子系统的维护者,Andrew Morton(akpm@linux-foundation.org)就是最后一道防线的维护者(maintainer of last resort)。

所有补丁默认都应抄送 linux-kernel@vger.kernel.org,但由于该列表流量巨大,不少开发者已经把它屏蔽。不过,请不要向无关的列表和无关的人滥发邮件。

许多与内核相关的列表托管在 kernel.org 上,完整列表见 https://subspace.kernel.org。不过,也有一些内核相关列表托管在其他地方。

Linus Torvalds 是所有进入 Linux 内核的改动的最终裁决者。他的邮箱是 <torvalds@linux-foundation.org>。他收到的邮件极多,如今只有极少数补丁由 Linus 直接经手,所以通常你应尽最大努力 -避免- 给他发邮件。

如果你的补丁修复了可被利用的安全漏洞,请把补丁发送到 security@kernel.org。对于严重 bug,可以考虑短暂的禁运(embargo),以便发行版厂商把补丁送到用户手中;显然,在这种情况下,补丁不应发送到任何公开列表。另见 Documentation/process/security-bugs.rst。

注:篇幅所限仅译核心章节,完整内容见原项目。
