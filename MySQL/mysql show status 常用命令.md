```
查询返回的行数
show status like '%innodb_rows_read%'

插入成功的行数
show status like '%innodb_rows_inserted%'

更新成功的行数
show status like '%innodb_rows_updated%'

删除成功的行数
show status like '%innodb_rows_deleted%'

查看锁
 SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;
show status like '%innodb_status%'
#show status like '%Table%'

查看慢查询
show status like '%Slow%'

查看运行时间
show status like '%up%'

查看锁的时间分布
show status like'%innodb_row_lock%';

执行select的计数
show status like '%Com_select%'

执行insert的计数，批量插入算一次
show status like '%Com_insert%'

执行更新操作的计数
show status like '%Com_update%'

执行删除操作的计数

show status like '%Com_delete%'

提交事务计数
show status like '%Com_commit%'

回滚事务计数
show status like '%Com_rollback%'

查看警告信息
show warnings
```
