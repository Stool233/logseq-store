- Tags: [[TLA+]] [[Paxos]]
-
- Paxos Revisited — The Synod
	- 使用Paxos构建复制状态机依赖于一个核心算法:single-decree synod(单一法令主教会议)算法,该算法使参与者能够就单个值达成共识。
		- 然后可以扩展这个核心算法,以便就一系列值达成共识,这些值可以用作复制状态机的命令日志。
	- 我们将首先从基本算法开始,并在后续文章中介绍它的扩展部分。
		- 我们将介绍的算法是原始Paxos论文2.3节中描述的基本协议。
	- 协议中的每个参与者都拥有一个账本,其目标是记录已达成共识的单个值。
		- 账本的"背面"也用于存储算法状态的某些重要信息;
		- 而"纸张"则用于存储那些可以丢失的不太重要的信息。
	- 参与者之间通过发送消息来通信,但由于使用的信使不太可靠,可能会导致消息丢失、重排序和重复。
		- 不过这些信使是可信的:他们不会篡改消息。
	- 每个参与者都会尝试让自己的值被其他人选择，同时也会接受其他人提出的值。最终，只能就一个单一的值达成共识。
	- 这个过程可以非正式地分解为以下几个阶段来描述：
		- 1. 参与者首先要试图让一定数量的参与者（法定人数）承诺接受一个特定的投票，
			- 该投票由唯一的编号标识。
			- （A participant first tries to get a quorum of participants to commit to a given ballot, identified by a unique number.）
		- 2. 一旦集齐了法定人数，参与者将选择一个值让这个法定人数的成员进行投票。
			- 选择的方式是：
				- 要么采用法定人数中编号最高的先前投票值，
				- 要么在没有先前投票的情况下选择任意值。
			- 做出选择后，参与者将请求法定人数中的每个成员对所选的值进行投票。
			- （Once a quorum has been gathered, the participant will choose a value for the quorum to vote on. The choice is made by taking the previous vote with the highest number in the quorum, or, if there were no previous votes, by taking any value. After making this choice, the participant will ask each member of the quorum to vote on the chosen value.）
		- 3. 当参与者收到上述第2步发出的提议时，除非它已经承诺了一个更高编号的投票，否则它将对该提议进行投票。
			- 成功的投票结果会被传达回提议的参与者。
			- 需要注意的是，参与者在投票后仍然可以承诺新的投票，但在这种情况下，之前的投票将会被考虑在新投票提议的选择中（如上述第2点所述）。
			- （When a participant receives a proposal issued by 2 above, it will vote on it, unless it has already committed to a higher numbered ballot. A successful vote is communicated back to the participant who proposed it. Note that a participant can still commit to a new ballot **after having voted**, but that then the previous vote will be taken account in the choice of value to be proposed for that new ballot(2 above).）
		- 4. 只有当法定人数中的所有成员都回复了肯定的投票后，提议的参与者才会将该值写入其账本。
			- （The proposing participant will only write the value to its ledger if all members of the quorum reply with a positive vote.）
		- 5. 一旦参与者在其账本中写入了一个值，它就会将这个信息传达给所有其他参与者，其他参与者也会将该值写入他们的账本中。
			- （Once a participant writes a value in its ledger, it communicates this to all other participants, who will write it in their ledger as well.）
	- 在这个非正式且相当模糊的描述之后，我们将直接切入正题，通过编写TLA+规范，为规范定义安全性属性，并尝试找到一个能推导出该安全性属性的归纳不变量。
-
-