> 🌐 本文档由 [torvalds/linux](https://github.com/torvalds/linux) 翻译,英文原版见原项目。

.. _stable_kernel_rules:

关于 Linux -stable 稳定版发布,你想知道的一切
===============================================================

哪些补丁可以被接受进入 "-stable" 稳定版分支,哪些不能,规则如下:

- 该补丁或等效的修复必须已存在于 Linux 主线(mainline,即上游 upstream)中。
- 它必须明显正确,并且经过测试。
- 连同上下文在内,它不能超过 100 行。
- 它必须遵循
  :ref:`Documentation/process/submitting-patches.rst <submittingpatches>`
  中的规则。
- 它必须修复一个确实困扰用户的真实缺陷,或者仅仅添加一个设备 ID。
  对前者进一步展开:

  - 它修复诸如 oops、挂死(hang)、数据损坏、真实安全问题、硬件 quirk、
    编译错误(但不适用于标记为 CONFIG_BROKEN 的特性)之类的问题,
    或某些"哦,这可不太妙"的问题。
  - 发行版内核(distribution kernel)用户报告的严重问题也可能被考虑,
    前提是它们修复了显著的性能或交互性问题。由于这类修复不够显而易见,
    且引入隐蔽回归的风险更高,因此只能由发行版内核维护者提交,并须附上
    补充说明:若存在对应的 bugzilla 条目则附上链接,并额外说明对用户
    可见的影响。
  - 不接受"这可能是个问题……"之类的东西,比如"理论上的竞态条件
    (race condition)",除非同时提供该缺陷如何被利用的说明。
  - 不接受对用户没有任何好处的"琐碎"修复(拼写修改、空白清理等)。


向 -stable 分支提交补丁的流程
----------------------------------------------------

.. note::

   安全类补丁不应(仅)由 -stable 审查流程处理,而应遵循
   :ref:`Documentation/process/security-bugs.rst <securitybugs>`
   中描述的流程。

向 -stable 分支提交变更共有三种方式:

1. 在你随后为进入主线而提交的补丁描述中加上"stable 标签(stable tag)"。
2. 请 stable 团队拣选一个已进入主线的补丁。
3. 向 stable 团队提交一个与已进入主线的变更等效的补丁。

下面各节将更详细地介绍这三种方式。

:ref:`option_1` 是**强烈**推荐的方式,它最简单也最常见。
:ref:`option_2` 主要适用于提交时未曾考虑向后移植(backport)的变更。
:ref:`option_3` 则是前两种方式的替代方案,适用于已进入主线的补丁需要
调整才能应用到较旧版本系列的情况(例如由于 API 变更)。

使用方式 2 或方式 3 时,你可以要求将你的变更纳入特定的 stable 版本系列。
这样做时,请确保该修复或等效修复在所有仍受支持的较新 stable 分支中同样
适用、已提交或已存在。这是为了避免用户日后升级时遇到回归,例如:为
5.19-rc1 合入的修复被向后移植到了 5.10.y,却没有移植到 5.15.y。

.. _option_1:

方式 1
********

若想让你为进入主线而提交的补丁日后被自动拣选进 stable 分支,请在签署
(sign-off)区域加入如下标签::

  Cc: stable@vger.kernel.org

修复尚未公开的漏洞时,请改用 ``Cc: stable@kernel.org``:发送到该地址的
邮件不会被投递到任何地方,因此可以降低通过 'git send-email' 意外公开
修复内容的风险。

补丁进入主线后,就会被应用到 stable 分支,作者或子系统维护者无需再做
任何额外操作。

如需向 stable 团队传达额外指令,可使用 shell 风格的行内注释来传递任意
或预定义的说明:

* 指定拣选(cherry-pick)该补丁所需的任何其他前置补丁::

    Cc: <stable@vger.kernel.org> # 3.3.x: a1f84a3: sched: Check for idle
    Cc: <stable@vger.kernel.org> # 3.3.x: 1b9508f: sched: Rate-limit newidle
    Cc: <stable@vger.kernel.org> # 3.3.x: fd21073: sched: Fix affinity logic
    Cc: <stable@vger.kernel.org> # 3.3.x
    Signed-off-by: Ingo Molnar <mingo@elte.hu>

  上述标签序列的含义等同于::

    git cherry-pick a1f84a3
    git cherry-pick 1b9508f
    git cherry-pick fd21073
    git cherry-pick <this commit>

  注意,对于一个补丁系列(patch series),不必把系列内部的补丁列为
  前置补丁。例如,假设你有如下补丁系列::

    patch1
    patch2

  其中 patch2 依赖 patch1;如果你已经把 patch1 标记为进入 stable,
  那么在 patch2 中就不必再把 patch1 列为前置补丁。

* 指明内核版本前置条件::

    Cc: <stable@vger.kernel.org> # 3.3.x

  该标签的含义等同于::

    git cherry-pick <this commit>

  即对从指定版本开始的每个 "-stable" 分支执行上述拣选。

  注意,如果 stable 团队能够从 Fixes: 标签推导出合适的版本,
  就无需这样标注。

* 延迟拣选补丁::

    Cc: <stable@vger.kernel.org> # after -rc3

* 指明已知问题::

    Cc: <stable@vger.kernel.org> # see patch description, needs adjustments for <= 6.3

此外还有一种 stable 标签变体,可用来让 stable 团队的向后移植工具
(例如 AUTOSEL 或查找含有 'Fixes:' 标签提交的脚本)忽略某个变更::

     Cc: <stable+noautosel@kernel.org> # reason goes here, and must be present

.. _option_2:

方式 2
********

如果补丁已被合入主线,请发送邮件到 stable@vger.kernel.org,内容包括:
补丁标题、commit ID、你认为它应当被应用的理由,以及你希望它被应用到
哪些内核版本。

.. _option_3:

方式 3
********

在确认补丁符合上述规则后,将其发送到 stable@vger.kernel.org,并说明你
希望它被应用到哪些内核版本。这样做时,必须在提交说明(changelog)中、
紧挨提交正文上方单独一行注明上游 commit ID,如下所示::

  commit <sha1> upstream.

或者也可以使用如下形式::

  [ Upstream commit <sha1> ]

如果提交的补丁与原始上游补丁存在偏差(例如因为必须针对较旧的 API 进行
调整),必须在补丁描述中非常清晰地记录并说明理由。


提交之后
------------------------

补丁被接受进入队列后,发送者会收到 ACK;被拒绝则会收到 NAK。根据
stable 团队成员的时间安排,这一回应可能需要几天时间。

一旦被接受,补丁将被加入 -stable 队列,供其他开发者和相关子系统
维护者审查。


审查周期
------------

- 当 -stable 维护者决定启动一轮审查周期时,补丁会被发送给审查委员会,
  以及受影响区域的维护者(除非提交者本人就是该区域维护者),并抄送
  (CC:)到 linux-kernel 邮件列表。
- 审查委员会有 48 小时时间对补丁给出 ACK 或 NAK。
- 如果补丁被委员会成员拒绝,或者 linux-kernel 成员提出维护者和成员们
  未曾意识到的问题并反对该补丁,补丁将被从队列中移除。
- 获得 ACK 的补丁将作为发布候选版本(-rc)的一部分再次发布,交由
  开发者和测试者测试。
- 通常只发布一个 -rc 版本;但如果仍有未解决的问题,一些补丁可能被
  修改或移除,也可能有新补丁进入队列。之后会发布更多 -rc 版本并继续
  测试,直到不再发现问题。
- 对 -rc 版本的反馈,可以通过邮件列表发送带有测试信息的 "Tested-by:"
  邮件来完成。这些 "Tested-by:" 标签会被收集起来,加入到发布提交中。
- 审查周期结束时,将发布新的 -stable 版本,包含队列中所有已通过测试
  的补丁。
- 安全补丁由内核安全团队直接合入 -stable 分支,不经过正常的审查周期。
  有关此流程的更多细节,请联系内核安全团队。


代码树
-----

- 补丁队列——既包括已完成版本的,也包括进行中版本的——可以在下面找到:

    https://git.kernel.org/pub/scm/linux/kernel/git/stable/stable-queue.git

- 所有 stable 内核最终定稿并打上标签的发布,按版本分别存放在不同分支中,
  位置如下:

    https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git

- 所有 stable 内核版本的发布候选可以在下面找到:

    https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable-rc.git/

  .. warning::
     -stable-rc 代码树只是 stable-queue 代码树在某一时刻的快照,会频繁
     变化,因此经常被 rebase。它只应用于测试目的(例如供 CI 系统使用)。


审查委员会
----------------

- 该委员会由多名自愿承担此项工作的内核开发者,以及少数并非自愿的开发者
  组成。
