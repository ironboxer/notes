### mysql在一个事务中是要先插入还是先更新 

在一个事务中有两个操作: insert and update, 从并发性能考虑, 如何安排两个操作的顺序?

=== start transction
1. insert
2. update
=== commit transction

从并发性能考虑, 要尽量减少锁的竞争。更新操作涉及到行锁的竞争，先插入再更新能够最大程度上减少事务时间的锁等待，提升并发度。
