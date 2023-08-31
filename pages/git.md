- Git 内部原理图解——对象、分支以及如何从零开始建仓库
  collapsed:: true
	- 来源：
		- https://www.freecodecamp.org/chinese/news/git-internals-objects-branches-create-repo/
	- Git 对象——blob、tree 和 commit
		- 将 `git` 看成一个文件系统（尤其是该系统的实时快照）是很有用的。
			- 一个文件系统从 *根目录（root directory）* 开始（在基于 UNIX 的系统中是 `/`），通常也会包含其它的目录（例如 `/usr` 或 `/bin`）。这些目录会包含其它的目录和（或）文件（例如 `/usr/1.txt`）。
		- 在 `git` 中，文件的内容存储在一些被称为 **blob** （二进制大对象）的对象中。
			- **blob** 与文件的不同在于，
				- 文件还会包含元数据（meta-data）。例如一个文件会“记住”它的创建时间，如果你把它移动到另一个目录，它的创建时间是不会改变的。
				- 相反，**blob** 只是内容——数据的二进制流。除了内容以外，**blob** 不会记录它的创建时间、名字或任何其它东西。
			- `git` 中的 **blob** 通过 [SHA-1 哈希值](https://en.wikipedia.org/wiki/SHA-1) 唯一标识。SHA-1 哈希值由 20 个字节（byte）组成，通常表示成 40 个十六进制形式的字符。
				- ![image.png](../assets/image_1691231828911_0.png)
		- 在 `git` 中，**树对象（tree）** 相当于目录。一个 **树对象** 基本上就是一个目录列表，它引用着 **blob** 和其它的 **树对象**。
			- **树对象** 也用 SHA-1 哈希值唯一标识，它通过其它对象（**blob** 或 **树对象**）的 SHA-1 哈希值引用它们。
				- ![image.png](../assets/image_1691231885848_0.png){:height 248, :width 658}
			- 这张图相当于一个文件系统，这个文件系统有一个根目录，根目录下有一个位于 `/test.js` 的文件和一个名为 `/docs` 的目录，`/docs` 目录下有两个文件：`/docs/pic.png` 和 `/docs/1.txt`。
				- ![image.png](../assets/image_1691232065372_0.png)
		- 在 `git` 中，一个快照就是一个 **提交（commit）**。
			- 即捕获该文件系统的一个快照，把那个时刻存在的所有文件连同它们的内容保存下来。
			- 一个 **提交** 对象包括一个指向主要 **树对象**（根目录）的指针和一些像 **提交者**、**提交信息** 和 **提交时间** 这样的元数据。
			- 在大多数情况下，一个 **提交** 还会有一个或多个父 **提交**——之前的快照。
			- 当然，**提交** 对象也通过它们的 SHA-1 哈希值唯一标识。
				- 这些哈希值就是我们使用 `git log` 命令时看到的那些哈希值。
			- ![image.png](../assets/image_1691232172985_0.png)
		- 每个 **提交** 都持有 *完整的快照*，并不只是与之前 **提交** 之前的差异。
			- 那么它是怎么工作的呢？难道它不代表我们每次提交都必须保存很多数据吗？
			- 让我们来看看改变一个文件的内容会发生什么。
				- ![image.png](../assets/image_1691232269475_0.png)
				- ![image.png](../assets/image_1691232281972_0.png)
				- ![image.png](../assets/image_1691232293959_0.png)
			- 几乎做好创建一个新 **提交** 对象的准备了，我们好像会再一次保存很多的数据——整个文件系统。但是真的有必要这么做吗？
				- 实际上，一些对象（尤其是 **blob** 对象）相比起之前的提交来说没有任何改变——**blob F92A0**仍然原封不动，**blob F00D1** 也一样。
			- 这就是其中的秘诀——只有对象改变了，我们才再次保存它。
				- 在这个例子中，我们不需要再次保存 **blob F92A0** 和 **blob F00b1**。我们只需要通过它们的哈希值引用它们，然后我们可以创建 **提交** 对象。
			- 由于这次 **提交** 不是第一次 **提交**，所以它有一个父节点——**commit A1337**。
				- ![image.png](../assets/image_1691232402251_0.png){:height 293, :width 624}
		- 让我们思考一下这些对象的哈希值吧。
			- 如果我写了 `git is awesome!` 并从它创建了一个 **blob**。你也在自己的系统上这么做，我们会有相同的哈希值吗？
				- 答案是肯定的。因为这两个 **blob** 有相同的内容，自然也会有相同的 SHA-1 哈希值。
			- 如果我创建了一个引用 `git is awesome!` 这个 **blob** 的 **树对象** ，赋给它一个特定的名字和元数据，你也在自己的系统上重复我的操作。我们会有相同的哈希值吗？
				- 答案还是肯定的。因为这两个 **树对象** 是相同的，它们会有同样的哈希值。
			- 如果我创建了一个指向那个 **树对象** 的 **提交对象**，提交信息为 `Hello`，你也在自己的系统上重复了一遍这个操作，结果会怎样呢？我们的哈希值还会相同吗？
				- 这个时候的答案是否定的。即使我们的 **提交对象** 指向了相同的 **树对象**，它们也会有不同的 **提交详情**——时间、提交者，等等。
	- Git 中的分支
		- **分支（branch）只不过是提交对象的命名引用**。
			- 我们可以一直用 SHA-1 哈希值引用一个 **提交**，但是人们通常喜欢以其他形式命名对象。
			- **分支** 恰好是引用 **提交** 的一种方式，实际上也只是这样。
		- 在大多数仓库中，主线开发都是在一个叫做 `master` 的分支上完成的。
			- `master` 只是一个名字，它是在我们使用 `git init` 命令的时候被创建的。
			- 正因为如此，它被广泛使用。
			- 然而，它并不特别，我们可以用任何我们喜欢的名字代替它。
		- 通常，分支指向的是当前开发线上的最近一次 **提交**。
			- ![image.png](../assets/image_1691232573426_0.png){:height 424, :width 449}
		- 我们通常使用 `git branch` 命令创建一个新分支，而我们实际创建的却是另一个指针（pointer）。
			- 假设我们使用 `git branch test` 命令创建了一个名为 `test` 的分支，我们实际上是创建了另一个指针，它指向当前分支上的同一 **提交**。
				- ![image.png](../assets/image_1691232709416_0.png){:height 610, :width 596}
		- `git` 是怎么知道我们当前所在的分支呢？答案是它维护了一个名为 `HEAD` 的特殊指针。
			- 通常情况下，`HEAD` 会指向一个分支，这个分支指向一个 **提交**。有时候，`HEAD` 也能直接指向一个 **提交**，不过这不是我们的重点。
				- ![image.png](../assets/image_1691232794600_0.png)
			- 活动分支（active branch）指的是我们当前所在的分支，也就是 `HEAD` 指向的分支。
			- 要将活动分支切换到 `test`，我们可以使用命令 `git checkout test`。现在我们已经能猜到这条命令真正做的事情了——它只不过是把 `HEAD` 指向的分支改成了 `test`。
				- ![image.png](../assets/image_1691232829626_0.png)
			- 在创建 `test` 分支之前，我们也可以使用 `git checkout -b test`，
				- 这条命令等价于先运行 `git branch test` 创建分支，
				- 再运行 `git checkout test` 使 `HEAD` 指向新的分支。
			- 如果我们做了一些改动并使用 `git commit` 创建了一个新 **提交** 呢？这个新 **提交** 会被添加到哪个分支上呢？
				- 答案是 `test` 分支，因为它是当前的活动分支（因为 `HEAD` 指向了它）。之后，`test` 指针会移动至新添加的 **提交** 上。注意 `HEAD` 仍然指向 `test`。
					- ![image.png](../assets/image_1691232877795_0.png)
				- 因此，如果我们使用 `git checkout master` 回到 master 分支，我们就让 `HEAD` 的再次指向 `master` 了。
					- ![image.png](../assets/image_1691232898417_0.png)
				- 如果我们现在创建一个新的 **提交**，它就会被添加到 `master` 分支，**commit B2424** 会成为新提交的父节点。
					- ![image.png](../assets/image_1691232916972_0.png)
	- 如何在 Git 中记录变化
		- 通常，我们在 **工作目录（working dir）** 中编写源代码。
			- **工作目录** （或 **工作树（working tree）**）可以是文件系统上的任何一个目录，它关联着一个 **仓库（repository）** 。
			- 目录内不仅包含工程的文件夹和文件，还包含一个名为 `.git` 的目录。稍后我们会再讨论 `git` 这个目录。
		- 在做了一些改动之后，我们想把这些改动记录到我们的 **仓库** 中。
			- 一个 **仓库** （缩写：**repo**）就是一系列 **提交** 的集合，每个 **提交** 都是工程 **工作树** 的归档。
			- 除了我们自己机器上的提交外，仓库也会包含他人机器上的提交。
			- **仓库** 也包含除代码文件以外的其它东西，例如 `HEAD` 指针、分支等等。
				- ![image.png](../assets/image_1691232992999_0.png)
		- 你可能使用过的其它和 `git` 类似工具，但是 `git` 并不会像其它工具那样直接将变化从 **工作树** 提交到 **仓库**。相反，它会先把这些变化注册到一个被称为 **索引（index）** 或 **暂存区（staging area）** 的地方。
			- 这两个术语指的都是同一个东西，它们也经常被 `git` 的文档使用，我们将会在这篇文章中交替使用它们。
		- 当我们 `checkout` 到一个分支时，`git` 会将上一次检出到工作目录中的所有文件填充到 **索引**，它们看起来就像最初被检出时的样子。之后执行 `git commit` 时， **提交** 会在当前 **索引** 的基础上创建。
		- **索引** 允许我们精心准备每次 **提交**。
			- 举个例子，自上一次 **提交** 以来，我们的 **工作目录** 中可能有两个文件发生了变化，但是我们可能只想将其中的一个添加到 **索引**（使用 `git add`），然后使用 `git commit` 记录这一个文件的变化。
				- ![image.png](../assets/image_1691233141517_0.png)
		- **工作目录** 下文件的状态不外乎有两种：**已跟踪（tracked）** 或 **未跟踪（untracked）**。
			- **已跟踪文件** 是指那些 `git` 已经知道的文件。它们要么已经在上一次快照（**提交**）中，要么已经被 **暂存（staged）**（换句话说，它们已经在 **暂存区** 中）。
			- **工作目录** 中除已跟踪文件以外的所有其它文件都属于 **未跟踪文件（untracked）**，它们既没有在上次快照（**提交**）中，也没有在 **暂存区** 中。
	- 如何设置   `.git`
		- **一个 git 仓库有两个主要组成部分：**
			- 一组对象——**blob**、**树对象** 和 **提交对象**。
			- 一个命名这些对象的方式——称为 **引用**。
			- 一个 **仓库** 可能还包含一些其它的东西，比如 git 钩子（hooks）。
				- 不过，仓库至少必须要有对象和引用。
		- **分支** 是引用的一种，`git` 内部将 **分支** 称为 **heads**，所以我们会为它们创建一个目录 `git\refs\heads`。
		- `HEAD` 如何实现的——它只是一个文件，文件内容描述了它所指向的分支。
	- 如何创建对象
		- 这个 **blob** 的哈希值为 `54f6...36`， `.git\objects` 下也多出来了一个名为 `54` 的目录，目录内有一个名为 `f6..36` 的文件。
			- 所以，`git` 实际上是使用 SHA-1 哈希值的前两个字符作为目录的名字，剩余字符用作 **blob** 所在文件的文件名。
			- 为什么要这样呢？考虑一个非常大的仓库，仓库的数据库内存有三十万个对象（**blob 对象**、**树对象** 和 **提交对象**）。从这三十万个哈希值中找出一个值会花些时间，因此，`git` 将这个问题划分成了 256 份。
			- 为了查找上面的那个哈希值，`git` 会先寻找 `.git\objects` 目录下名为 `54` 的目录，然后搜索那个目录，这进一步缩小了搜索范围。`.git\objects` 目录下最多可能会有 256 个子目录（从 `00` 到 `FF`）。
		- 创建 **blob** 这个过程通常发生在我们将一些东西添加到 **暂存区** 的时候——也就是我们使用 `git add` 的时候。
		-
-
- mac下模拟git hash-object
  collapsed:: true
	- ```
	  >echo -n "hello world" > test.txt
	  >git hash-object test.txt
	  95d09f2b10159347eece71399a7e2e907ea3df4f
	  >printf "blob %s\0" "$(stat -f "%z" test.txt)" | cat - test.txt | openssl dgst -sha1
	  (stdin)= 95d09f2b10159347eece71399a7e2e907ea3df4f
	  ```
- 根据hash获取文件路径
  collapsed:: true
	- ```
	  git rev-list --all --objects | grep <sha1-hash>
	  ```
- Git对象打包（Packfiles）：
  collapsed:: true
	- 为了节省空间和提高性能，Git 会定期把许多小的对象文件打包到一起，形成一个大的 "packfile"。打包后的对象不再以单独的文件形式存在于 `.git/objects/` 目录下，而是存在于 `.git/objects/pack/` 目录下的一个 packfile 中。你可以使用 `git verify-pack` 命令来查看 packfile 的内容。
	- 如果你需要查看某个具体对象的内容，你可以使用 `git cat-file` 命令，例如
		- ```
		  git cat-file -p 25fd71335b3ddbeaeedc9b869647edfc515173f7
		  ```
	- 如何查看hash在哪个packfile中
		- ```
		  for pack in .git/objects/pack/*.idx; do
		    git verify-pack -v $pack | grep 25fd71335b3ddbeaeedc9b869647edfc515173f7 && echo $pack
		  done
		  ```
	- Packfile 的原理可以分为以下几个部分：
		- **打包**：当 Git 需要创建一个新的 packfile 时（例如在执行 `git gc` 命令时），它会将许多小的对象打包成一个大的 packfile。这个过程包括查找和打包那些没有被其他 packfile 包含的对象。
		- **差分压缩**：在打包的过程中，Git 会尽量使用一种叫做 "差分压缩"（delta compression）的技术。这种技术的原理是，只存储一个对象相对于另一个对象的差异，而不是存储每个对象的完整内容。这种技术对于版本控制系统非常有效，因为在版本控制系统中，很多版本的文件都和其他版本的文件非常相似。
		- **索引**：每个 packfile 都有一个对应的索引文件（.idx 文件）。这个索引文件包含了 packfile 中每个对象的哈希值和在 packfile 中的位置。当 Git 需要查找一个对象时，它会首先查找这个对象的哈希值在哪个索引文件中，然后根据索引文件中的信息在对应的 packfile 中找到这个对象。
		- **解包**：当 Git 需要读取一个对象的内容时，如果这个对象在一个 packfile 中，Git 会首先在 packfile 的索引文件中查找这个对象的位置，然后在 packfile 中找到这个对象。如果这个对象使用了差分压缩，Git 会先找到这个对象的基对象，然后应用差异来重建这个对象的完整内容。
	- Git 的 `.idx` 文件设计得非常高效，即使在处理大量对象时也能保持高性能。有几个关键的设计选择使得 `.idx` 文件能够快速定位存储在 `.pack` 文件中的对象：
	-
		- **排序的哈希**：`.idx` 文件中的对象哈希是有序的，这允许 Git 使用二分查找算法来定位特定的对象，而二分查找是一个对数时间复杂度的操作。这意味着即使对象数量翻倍，查找时间只会增加一个常数。
		- **偏移表**：`.idx` 文件中的偏移表使得 Git 可以快速地找到对象在 `.pack` 文件中的位置。
		- **版本 2 的增强**：版本 2 的 `.idx` 文件引入了一些增强，包括对大于 4GB 的 `.pack` 文件的支持和 CRC32 校验和，这些都有助于提高 `.idx` 文件的性能和可靠性。
		- **内存映射文件**：Git 使用内存映射文件（memory-mapped files）来处理 `.idx` 文件。这意味着整个 `.idx` 文件会被映射到内存中，这使得访问 `.idx` 文件就像访问内存一样快。
-
- Git UNDO — how to rewrite Git history with confidence
	- 来源：
		- https://medium.com/@Omer_Rosenbaum/git-undo-how-to-rewrite-git-history-with-confidence-d4452e2969c2
	- Undoing the changes
		- git reset
		  collapsed:: true
			- `git reset --soft`
				- So the very last step you did before was to `git commit`, which actually means two things — Git created a commit object, and moved `main`, the active branch.
				- To undo this step, use the command `git reset --soft HEAD~1`.
					- This command asks Git to change whatever `HEAD` is pointing to.
					- However, this command did **not **affect the state of the index or the working tree.
				- ![image.png](../assets/image_1693459074462_0.png)
			- `git reset --mixed`
				- (note: `--mixed` is the default switch for `git reset`).
				- This command starts the same as `git reset --soft HEAD~1`. Meaning it takes the pointer of whatever `HEAD` is pointing to now, which is the `main` branch, and sets it to `HEAD~1`
				- Next, Git goes further, effectively undoing the changes we made to the index.
					- That is, changing the index so that it matches with the current `HEAD`, the new `HEAD` after setting it in the first step.
				- ![image.png](../assets/image_1693460161497_0.png)
			- `git reset --hard`
				- Go ahead and run `git reset --hard HEAD~1`
					- Again, Git starts with the `--soft` stage, setting whatever `HEAD` is pointing to (`main`), to `HEAD~1` (“Commit 1”).
					- Next, moving on to the `--mixed` stage, matching the index with `HEAD`. That is, Git undoes the staging of `2.txt`.
					- It is time for the `--hard` step, where Git goes even further and matches the working dir with the stage of the index.
						- In this case, it means **removing `2.txt` also from the working dir.**
						- (Note: in this specific case the file is *untracked* so it won’t be deleted from the file system, it isn’t really important in order to understand `git reset` though)
							- 在Git中，"untracked file"指的是那些存在于你的工作目录中，但并未被纳入Git追踪的文件。换句话说，这些文件在你的Git仓库的历史记录中没有出现过。这可能是因为这些文件是新创建的，或者是在`.gitignore`文件中明确指定不进行追踪的。
							- 当你执行`git reset --hard`命令时，Git会重置你的工作目录和索引(index)以匹配HEAD。但是，此命令不会影响未跟踪的文件。因此，如果一个文件是"untracked"的，即使你执行了`git reset --hard`，该文件仍然会保留在你的工作目录中。
					- ![image.png](../assets/image_1693460405480_0.png)
			-
		- git cherry-pick
		  collapsed:: true
			- This command takes the changes introduced in the specified revision, and apply them to the active commit.
			- It also creates a new commit object, and updates the active branch to point to this new object.
			- ![image.png](../assets/image_1693494555351_0.png)
			-
			- ![image.png](../assets/image_1693494224085_0.png)
			- In the example above I specified the SHA-1 identifier of the created commit, but you could also use `git cherry-pick main`, as the commit whose changes we are applying is the one `main` is pointing to.
			- Note that `git cherry-pick` actually computes the difference between the specified commit and its parent, and then applies them on the active commit.
				- This means that sometimes, Git won’t be able to apply those changes as you may get a conflict, but that’s a topic for another post.
				- Also note that you can ask Git to `cherry-pick` the changes introduced in any commit, not only commits referenced by a branch.
		- git revert
			- ![image.png](../assets/image_1693494587604_0.png)
			- ![image.png](../assets/image_1693494598942_0.png)
			- This command takes the commit you’re providing it with, compute the Diff from its parent commit, just like `git cherry-pick`, but this time it computes the reverse changes.
				- So if in the specified commit you added a line, the reverse would delete the line, and vice versa.
			- `git revert` created a new commit object, which means it’s an addition to the history.
				- By using `git revert` you didn’t rewrite history.
				- You admitted your past mistake, and this commit is an acknowledgement that you made had a mistake and now you fixed it.
				-
-