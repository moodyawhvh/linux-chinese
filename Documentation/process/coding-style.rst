> 🌐 本文档由 [torvalds/linux](https://github.com/torvalds/linux) 翻译,英文原版见原项目。

.. _codingstyle:

Linux 内核编码风格
==================

本文是一份简短的文档,描述 Linux 内核推荐的编码风格。编码风格是非常个人化的东西,我不会把我的观点**强加**给任何人,但凡是我必须维护的东西,一律按这里所说的风格来,我也希望大多数其他代码同样如此。请至少认真考虑一下本文提出的这些要点。

首先,我建议你打印一份 GNU 编码规范,然后**不要**去读它。把它们烧掉,这是一个很棒的象征性举动。

好了,正文开始:


1) 缩进
--------------

制表符(tab)是 8 个字符,因此缩进也是 8 个字符。有一些异端运动试图把缩进改成 4(甚至 2!)个字符深,这就好比试图把圆周率 π 定义成 3。

理由:缩进的全部意义,在于清晰地界定一个控制块的开始与结束。尤其是当你已经连续盯着屏幕看了 20 个小时之后,你会发现:缩进越大,缩进的层次结构就越容易看清。

现在,有些人会声称 8 字符缩进会让代码向右挪得太远,在 80 列的终端屏幕上难以阅读。对此的回答是:如果你需要的缩进层级超过 3 层,那你反正已经完蛋了,应该先修好你的程序。

简而言之,8 字符缩进让代码更易读,还有一个额外的好处:当你把函数嵌套得太深时,它会向你发出警告。请重视这个警告。

在 switch 语句中减少多级缩进的首选方式,是把 ``switch`` 和它下属的 ``case`` 标签对齐在同一列上,而不是对 ``case`` 标签做 ``double-indenting``(双重缩进)。例如:

.. code-block:: c

	switch (suffix) {
	case 'G':
	case 'g':
		mem <<= 30;
		break;
	case 'M':
	case 'm':
		mem <<= 20;
		break;
	case 'K':
	case 'k':
		mem <<= 10;
		fallthrough;
	default:
		break;
	}

除非你有什么东西想隐瞒,否则不要把多条语句塞在同一行里:

.. code-block:: c

	if (condition) do_this;
	  do_something_everytime;

不要用逗号来逃避使用大括号:

.. code-block:: c

	if (condition)
		do_this(), do_that();

多条语句时一律使用大括号:

.. code-block:: c

	if (condition) {
		do_this();
		do_that();
	}

同样,也不要把多个赋值语句放在同一行。内核编码风格极其简单,请避免晦涩机巧的表达式。


在注释和文档之外,并且除 Kconfig 以外的场合,绝不用空格做缩进;上面那个示例就是故意写错的。

换一个像样的编辑器,并且不要在行尾留下空白字符。


2) 长行与长字符串的折行
----------------------------------

编码风格的核心,是让代码借助常用工具获得可读性与可维护性。

单行长度的首选上限是 80 列。

超过 80 列的语句应当拆分成有意义的片段,除非超出 80 列能显著提升可读性并且不隐藏任何信息。

后继行总是比父行明显更短,并且明显靠右放置。一个非常常用的风格,是把后继行对齐到函数开括号的位置。

参数列表很长的函数头同样适用这些规则。

但是,绝不要折行用户可见的字符串(例如 printk 消息),因为那会破坏 grep 它们的能力。


3) 大括号与空格的摆放
----------------------------

C 风格里另一个永远争论不休的问题是大括号的摆放。与缩进宽度不同,选择这种还是那种摆放策略并没有多少技术上的理由,但正如先知 Kernighan 和 Ritchie 向我们启示的那样,首选方式是:把开括号放在行尾,把闭括号放在行首,像这样:

.. code-block:: c

	if (x is true) {
		we do y
	}

这条规则适用于所有非函数的语句块(if、switch、for、while、do)。例如:

.. code-block:: c

	switch (action) {
	case KOBJ_ADD:
		return "add";
	case KOBJ_REMOVE:
		return "remove";
	case KOBJ_CHANGE:
		return "change";
	default:
		return NULL;
	}

不过,存在一种特殊情况,也就是函数:函数的开括号要放在下一行的行首,如下:

.. code-block:: c

	int function(int x)
	{
		body of function
	}

全世界的异端分子都声称这种不一致是……嗯……很不一致的,但所有思想正确的人都知道:(a) K&R 是**对的**,(b) K&R 是对的。再说,函数本来就很特殊(C 语言里函数不能嵌套)。

注意,闭括号单独占一行且该行不写任何东西,**除非**它后面紧跟同一条语句的延续部分,即 do 语句中的 ``while``,或 if 语句中的 ``else``,像这样:

.. code-block:: c

	do {
		body of do-loop
	} while (condition);

以及

.. code-block:: c

	if (x == y) {
		..
	} else if (x > y) {
		...
	} else {
		....
	}

理由:K&R。

另外,这种大括号摆法还能把空行(或近乎空行)的数量降到最少,而不损失任何可读性。于是,既然屏幕上的换行资源并非可再生资源(想想只有 25 行的终端屏幕吧),你就能省出更多空行来写注释。

当单条语句就够用时,不要多此一举地加大括号。

.. code-block:: c

	if (condition)
		action();

以及

.. code-block:: c

	if (condition)
		do_this();
	else
		do_that();

但如果条件语句的分支中只有一个是单条语句,这条规则就不适用;在后一种情况下,两个分支都要加大括号:

.. code-block:: c

	if (condition) {
		do_this();
		do_that();
	} else {
		otherwise();
	}

同样,当循环体包含的不止一条简单语句时,也要使用大括号:

.. code-block:: c

	while (condition) {
		if (test)
			do_something();
	}

3.1) 空格
***********

Linux 内核的空格使用风格(大部分)取决于其对象是函数还是关键字。(大多数)关键字后面要使用一个空格。值得注意的例外是 sizeof、typeof、alignof 和 __attribute__,它们看起来有点像函数(在 Linux 中通常也带括号使用,尽管语言本身并不要求,例如在声明 ``struct fileinfo info;`` 之后,可以写 ``sizeof info``)。

因此,在这些关键字之后要使用一个空格::

	if, switch, case, for, do, while

但 sizeof、typeof、alignof 或 __attribute__ 后面不要加空格。例如:

.. code-block:: c


	s = sizeof(struct file);

不要在括号围起来的表达式周围(内侧)添加空格。下面这个例子是**糟糕**的:

.. code-block:: c


	s = sizeof( struct file );

声明指针数据或返回指针类型的函数时,``*`` 的首选用法是紧挨着数据名或函数名,而不是紧挨着类型名。例如:

.. code-block:: c


	char *linux_banner;
	unsigned long long memparse(char *ptr, char **retptr);
	char *match_strdup(substring_t *s);

大多数二元与三元运算符两侧各留一个空格,例如下面这些::

	=  +  -  <  >  *  /  %  |  &  ^  <=  >=  ==  !=  ?  :

但一元运算符后面不加空格::

	&  *  +  -  ~  !  sizeof  typeof  alignof  __attribute__  defined

后缀自增与自减一元运算符前面不加空格::

	++  --

前缀自增与自减一元运算符后面不加空格::

	++  --

结构体成员运算符 ``.`` 和 ``->`` 两边都不要加空格。

不要在行尾留下尾随空白。一些带 ``smart``(智能)缩进功能的编辑器会在新行的行首插入适量的空白,让你能立刻开始输入下一行代码;但其中一些编辑器在你最终没有在那里输入代码时(比如留下一个空行)并不会移除这些空白。结果就是,你会得到一些带尾随空白的行。

Git 会对引入尾随空白的补丁发出警告,并且可以按需帮你剥掉尾随空白;然而,如果你在应用一个补丁系列,这样做会改变上下文行,可能导致该系列中后面的补丁应用失败。


4) 命名
---------

C 是一门斯巴达式的语言,你的命名规范也应该如此。与 Modula-2 和 Pascal 程序员不同,C 程序员不会使用 ThisVariableIsATemporaryCounter 这种“可爱”的名字。C 程序员会把这样的变量叫做 ``tmp``,这个名字写起来容易得多,而理解起来丝毫不难。

不过,虽然大小写混用的名字让人皱眉,全局变量却必须有描述性的名字。把一个全局函数叫成 ``foo`` 是该吃枪子的重罪。

全局变量(只有在你**真的**非用不可时才用)需要有描述性的名字,全局函数同理。如果你写了一个统计活跃用户数量的函数,就应该叫它 ``count_active_users()`` 或类似的名字,而**不要**叫它 ``cntusr()``。

把函数的类型信息编码进函数名(即所谓的匈牙利命名法)是愚蠢的做法——编译器反正知道这些类型,而且会帮你检查,这种做法只会把程序员搞糊涂。

局部变量名应当简短而切中要害。如果你有一个随手用的整数循环计数器,把它叫做 ``i`` 大概就最合适。在没有被误解之虞的情况下,叫它 ``loop_counter`` 毫无产出。类似地,``tmp`` 可以是任何用来存放临时值的变量,无论其类型是什么。

如果你会害怕把自己的局部变量名搞混,那你还有另一种毛病,叫做“函数生长荷尔蒙失调综合征”(function-growth-hormone-imbalance syndrome)。参见第 6 章(函数)。

对于符号名和文档,避免引入 'master / slave'(或不与 'master' 搭配而单独使用的 'slave')以及 'blacklist / whitelist' 的新用法。

'master / slave' 推荐的替代词:
    '{primary,main} / {secondary,replica,subordinate}'
    '{initiator,requester} / {target,responder}'
    '{controller,host} / {device,worker,proxy}'
    'leader / follower'
    'director / performer'

'blacklist/whitelist' 推荐的替代词:
    'denylist / allowlist'
    'blocklist / passlist'

允许引入新用法的例外情形是:为了维护用户空间 ABI/API,或者更新既有的(截至 2020 年)硬件或协议规范的代码,而这些规范强制要求使用那些术语。对于新规范,应尽可能把规范中的此类术语转译为内核编码标准中的对应说法。

5) Typedefs
-----------

请不要使用 ``vps_t`` 这类东西。

对结构体和指针使用 typedef 是一个**错误**。当你在源码中看到

.. code-block:: c


	vps_t a;

时,你能说出它是什么意思吗?
相反,如果写的是

.. code-block:: c

	struct virtual_container *a;

你就能真正看出 ``a`` 是什么。

很多人以为 typedef 能“提高可读性”。并非如此。typedef 只在下列情况下才有用:

 (a) 完全不透明的对象(此时 typedef 被刻意用来**隐藏**对象的真实身份)。

     例如:``pte_t`` 等不透明对象,你只能借助适当的访问器函数(accessor functions)来访问它们。

     .. note::

       不透明性和 ``accessor functions``(访问器函数)本身并不是什么好事。我们之所以对 pte_t 这类东西保留它们,是因为那里确实**完全**不存在任何可移植地可访问的信息。

 (b) 清楚明确的整数类型,此时这种抽象**有助于**避免弄不清它究竟是 ``int`` 还是 ``long``。

     u8/u16/u32 是完全没问题的 typedef,尽管把它们归入类别 (d) 比归入这里更贴切。

     .. note::

       再强调一次——这样做必须有一个**理由**。如果某个东西本来就是 ``unsigned long``,那就没有理由再来这么一下

	typedef unsigned long myflags_t;

     但如果有明确的理由,说明它在某些情况下可能是 ``unsigned int``、而在其他配置下可能是 ``unsigned long``,那就尽管去用 typedef。

 (c) 当你用 sparse 真正地创建一个用于类型检查的**新**类型时。

> 注:篇幅所限仅译核心章节,完整内容见原项目。
