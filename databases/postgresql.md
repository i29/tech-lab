# PostgreSQL



```sql
# 连接 PostgreSQL
psql -U user_name -d database_name -h serverhost

# 查看帮助： help
\h #查看所有的sql关键字
\? #命令行操作的帮助
\d #查看当前schema 中所有的表
\q #退出pg命令行
\d #schema.table 查看表的结构
\x #横纵显示切换
\dT+ #显示扩展类型相关属性及描述
\l #列出所有的数据库
\timing #显示执行时间
\c database_name #切换数据库
set search to schema #切换schema
explain sql #解释或分析sql执行过程
```

```sql
# 更新数据，`||` 连接字符串
UPDATE tmpdb.tmptable SET "uri" = 'https://test.org/topic/' || token_id
```