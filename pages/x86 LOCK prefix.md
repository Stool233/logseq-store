- https://hackmd.io/@vesuppi/Syvoiw1f8
- Cache coherence protocol and atomicity（缓存一致性协议和原子性）
	- Cache coherence protocol can guarantee atomicity of a single cache line operation like `ADD`（缓存一致性协议可以保证像ADD这样的单个缓存行操作的原子性）
		- imaging an implementation as follows:
			- Before executing the `ADD`, the cache will broadcast INVALIDATE on the bus (supposed its state was `shared` before).
			- On this bus, only one message can be broadcast each time.
			- So other processors’ copies will be Inv.
			- If another process broadcast another INV message immediately after that, the `XADD` instruction won’t respond until it finishes.
		- Suppose `A` and `B` are trying to do `ADD` at the same time:
			- Somehow `A` sends out the INV message first, so the cache line states are
				- `A`: shared → modified
				- `B`: shared → invalidated
			- Now `B` gets the bus and sends out its READX message, which requires a copy of the modified value and also invalidate all others.
			- When `A` gets the message, `ADD` won’t be interrupted. It only returns the updated value and invalidates itself once `ADD` is done.
- Lock signal and bus lock
	- Intel 64 and IA-32 processors provide a LOCK# signal that is asserted automatically during certain critical memory operations to lock the system bus or equivalent link.
		- While this output signal is asserted, requests from other processors or bus agents for control of the bus are blocked.
	- In a multiprocessor environment, the LOCK# signal ensures that the processor has exclusive use of any shared memory while the signal is asserted.
	- Beginning with the P6 family processors, when the LOCK prefix is prefixed to an instruction and the memory area being accessed is cached internally in the processor, the LOCK# signal is generally not asserted. Instead, only the processor’s cache is locked.
		- Here, the processor’s cache coherency mechanism ensures that the operation is carried out atomically with regards to memory. See “Effects of a Locked Operation on Internal Processor Caches” in Chapter 8 of Intel® 64 and IA-32 Architectures Software Developer’s Manual, Volume 3A, the for more information on locking of caches.
- Why without LOCK inc won’t be atomic
	- On modern CPUs, the LOCK prefix locks the cache line so that the read-modify-write operation is logically atomic. These are oversimplified, but hopefully they’ll give you the idea.
	- Unlocked increment:
		- 1. Acquire cache line, shareable is fine. Read the value.
		- 2. Add one to the read value.
		- 3. Acquire cache line exclusive (if not already E or M) and lock it.
		- 4. Write the new value to the cache line.
		- 5. Change the cache line to modified and unlock it.
	- Locked increment:
		- 1. Acquire cache line exclusive (if not already E or M) and lock it.
		- 2. Read value.
		- 3. Add one to it.
		- 4. Write the new value to the cache line.
		- 5. Change the cache line to modified and unlock it.
	- Notice the difference?
		- In the unlocked increment, the cache line is only locked during the write memory operation, just like all writes.
		- In the locked increment, the cache line is held across the entire instruction, all the way from the read operation to the write operation and including during the increment itself.
	- Remember, the “LOCK” prefix is implemented by the MESI protocol (or something more complicated, like MESIF on Intel or MEOSI on AMD) in reality.
		- Even then, modern processors probably implement something far more complicated, and its unfortunate that its undocumented.
		- Still, LOCK swap is clearly more efficient under MESI than LOCK cmpxchg.
- [https://en.wikipedia.org/wiki/MSI_protocol](https://en.wikipedia.org/wiki/MSI_protocol)
  [https://www.felixcloutier.com/x86/lock](https://www.felixcloutier.com/x86/lock)
-