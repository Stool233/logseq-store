- WAL，全称为 Write-Ahead Logging，是一种用于数据库和文件系统的标准技术，旨在确保即使在系统故障（如崩溃或电源故障）后，也能保持数据的完整性和一致性。
- 工作原理
	- WAL 的基本思想是，在对数据库中的数据进行任何更改之前，先将这些更改记录到一个持久化的日志中。
	- 这个日志通常被称为 WAL 文件或日志文件。
	- 在发生故障并随后重新启动系统时，这些日志可以被用来重做（redo）或取消（undo）在崩溃发生前的操作，恢复数据库到一个一致的状态。
- WAL 的关键步骤
	- 1. **开始事务**：当一个事务开始时，它的所有操作都会先被写入 WAL，而不是直接应用到数据库文件。
	- 2. **写入日志**：每个数据修改操作都会生成一个或多个日志记录，并且这些记录会被追加到 WAL 文件中。
	- 3. **日志刷写**：日志记录会定期从内存刷写到磁盘上的 WAL 文件中。这个操作可以是同步的，也可以是异步的，具体取决于系统的配置。
	- 4. **提交事务**：只有当一个事务相关的所有日志记录都安全地写入到 WAL 文件后，事务才算提交成功。
	- 5. **数据写入**：在后台，数据库管理系统（DBMS）会将这些更改渐进式地应用到数据库文件中，这个过程称为 checkpointing。
	- 6. **恢复过程**：如果系统崩溃，DBMS 在重启时会检查 WAL，应用那些已经成功写入日志但尚未写入数据库文件的记录。
- WAL 的优点
	- **数据完整性**：即使在系统崩溃的情况下，也能确保数据的一致性和完整性。
	- **恢复能力**：提供了一种机制来恢复到最后一次一致的状态，而无需重新执行所有事务。
	- **并发和性能**：WAL 可以减少对数据库文件的写入操作，因为数据的实际写入可以在较低的负载时异步进行，从而提高了并发性能。
	- **原子性**：支持事务的原子性，确保事务要么完全应用要么完全不应用。
- WAL 的缺点
	- **日志管理**：需要定期维护和清理 WAL 文件，否则日志文件可能会变得非常大。
	- **写入延迟**：由于所有更改都必须首先写入 WAL，这可能会导致比直接写入数据库文件更大的延迟。
-