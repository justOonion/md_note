## Oracle 创建 DBLink 的方法
```sql
create database link {{dblinkName}}
 connect to {{dbName}} identified by {{dbPassword}}
  using '(
  				DESCRIPTION =(
  					ADDRESS_LIST =(
  						ADDRESS =(PROTOCOL = TCP)(HOST = {{HOST}})(PORT = {{PORT}})
  					)
  				)
  				(
            CONNECT_DATA =(SERVICE_NAME = {{SERVICE_NAME}})
        	)
  			)';
```

**HOST** ： 表示远程数据库IP
**PORT** ： 表示远程数据库端口
**SERVICE_NAME** ： 远程数据库的实例名


### 使用
```sql
select * from db.tb_test@TestDblink;
```

### 查看所有的DBLink
```sql
select owner,object_name from dba_objects where object_type='DATABASE LINK';
```

### 删除DBLink
```sql
drop database link TestDblink;
```

### 创建同义词
```sql
create synonym T_DC_BILL_LIST for DC_BILL.T_DC_BILL_LIST;
create synonym T_DC_BILL_HEAD for DC_BILL.T_DC_BILL_HEAD;
```





### oracle解锁

``` sql
select s.machine sourse_host,p.SPID PID,l.session_id sid,s.serial#,l.locked_mode,l.oracle_username,s.user#,l.os_user_name,s.terminal,a.sql_text,a.action from
v$sqlarea a,v$session s, v$locked_object l,sys.v_$process p
where l.session_id=s.sid and s.PREV_SQL_ADDR=a.address and s.PADDR=p.addr order by sid,s.serial#;

alter system kill session 'sid,serial#';
```

### 表、列、索引等元数据
``` sql
/*表*/
select table_name, tablespace_name, status, sample_size, temporary 
from user_tables 
where table_name = 'T_ERP_DEC_E_HEAD';
/*列*/
select table_name, column_name, data_type, data_length, nullable, 
from USER_TAB_COLUMNS
where TABLE_NAME = 'T_ERP_DEC_E_HEAD';
/*表描述*/
select table_name, column_name, comments 
from user_tab_comments 
where table_name = 'T_ERP_DEC_E_HEAD';
/*列描述*/
select table_name, column_name, comments 
from user_col_comments 
where table_name = 'T_ERP_DEC_E_HEAD';
/*索引*/
select index_name, index_type, table_name, uniqueness 
from user_indexes 
where table_name = 'T_DEC_ERP_E_HEAD_N';
/*索引列
	联合索引对应 多条记录
*/
select index_name, table_name, column_name, column_position 
from user_ind_columns 
where index_name = 'IDX_DEC_ERP_E_HEAD_N_1';
/* 获取表的约束 */
select * from user_constraints where table_name='table_name'
```

### oracle 输出1-10
``` sql
select level from dual connect by level<11
```

### 查oracle 操作日志
```sql
-- 查询数据库执行sql记录
select t.sql_text, t.FIRST_LOAD_TIME
from v$sqlarea t
where t.sql_text like '%table_name%'
order by t.FIRST_LOAD_TIME desc;
-- 数据库启动记录
select t.database_status, t.instance_name, t.startup_time, t.host_name from v$instance t;
```



### oracle根据约束名查找 哪张表 哪个字段约束信息

```sql
SELECT A.CONSTRAINT_NAME,A.TABLE_NAME,A.COLUMN_NAME,B.CONSTRAINT_TYPE FROM USER_CONS_COLUMNS A, USER_CONSTRAINTS B WHERE A.CONSTRAINT_NAME =B.CONSTRAINT_NAME and a.constraint_name LIKE '%SYS_C00351160%'
```



### merge into的用法

```sql
-- 主要用于批量更新场景（存在update，不存在insert）
merge into T_MAIN_TABLE m
using T_OTHER_TABLE o
on (m.fKey = o.pKey)
WHEN MATCHED THEN
    update
        set m.col1 = o.col1
            m.col2 = o.col2
    where m.sid in ('id1','id2')
WHEN NOT MATCHED THEN
	  insert(cols...) values (vals...);
```

### 数据恢复
```sql
alter table T_ERP_BOM_ORIGINAL enable row movement;
flashback table cfg_business_adapter to timestamp to_timestamp('20160923 10:00:00','YYYYMMDD HH24:MI:SS'); 
```

### 创建用户
```sql
--创建 gwstd_htt 用户
  create user gwstd_htt
  identified by "eport"
  default tablespace USERS
  temporary tablespace TEMP
  profile DEFAULT;

grant all privileges to gwstd_htt;
```