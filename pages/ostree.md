- twitter分享
  collapsed:: true
	- {{twitter https://twitter.com/hemashushu/status/1761924476135194673/photo/1}}
- ostree —— 用 [[gi]]t 的方式更新系统
- 熟悉git和linux的同学可能会想过一个问题：能不能用git的方式来更新系统呢？
	- 包括建立分支、回滚等功能，用在操作系统会如何？
- 而 [#ostree](https://twitter.com/hashtag/ostree?src=hashtag_click) 则是这种想法的实践。它的实现方法非常简单却巧妙。
	- 首先所有文件计算出sha值然后以该值作为文件名储存在本地repo里
		- 可预想，内容相同的文件只会存储一份数据在repo里，
	- 第二步，把每个目录里头的文件的名称和sha值构成一个列表，把列表以其内容的sha值作为文件名也储存在repo里。所以最后会得到一棵树，树根就是根目录的sha值。
		- ![](https://pbs.twimg.com/media/GHOZSuYbQAAj1ty?format=png&name=900x900)
		-
	- 系统更新时，树里头的任一个文件发生改变，都会产生一个新的根节点。而commit则是（续
- 接）这些根节点的值的列表。checkout时就会把这颗树的文件做硬链接到文件系统里。ostree还包含一个系统启动引导器，用于选择根节点作为启动，比如系统更新失败后导致启动不了，则可以启动旧的一个commit。
- ostree除了/etc和/var目录之外的是只读的，所以在使用体验上可能跟传统的发行版不太一样（续
- 接）如果有兴趣玩一下ostree的系统，可以试试 gnome os ([https://os.gnome.org](https://t.co/GgKLwvfnj9))
  如果好奇ostree的原理，可以看 [https://ostreedev.github.io/ostree/](https://t.co/5U6pEI77tc)
  其实 flatpak 也是基于 ostree 的，感兴趣的到 [https://docs.flatpak.org/en/latest/](https://t.co/uDMqn95p99) 文档写得很清晰，跟着做就能制作自己的flatpak应用程序安装包和本地repo
- Silverblue 就是基于这个的吧，不过我还是更喜欢 [[NixOS]] 的 OS [[as Code]].
	- 是的，Fedora Silverblue 也是用到它
- [[不可变系统]]