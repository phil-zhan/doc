# work-Hive

## hive导出文件 {id="hive_1"}

```SQL
---- step1: 新建一个临时表，标的格式化需要是text，分隔符可以自定义，格式如下
CREATE table if NOT EXISTS tmp_zfw.tmp_111 (
  ip STRING COMMENT "源IP",
  c STRING COMMENT "IP对应PTR格式的C段",
  b STRING COMMENT "IP对应PTR格式的B段",
  a STRING COMMENT "IP对应PTR格式的A段"
)
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ','
COLLECTION ITEMS TERMINATED BY '_'
MAP KEYS TERMINATED BY ':' 
STORED AS TEXTFILE

---- step2: 将要导出的数据写到临时表


---- step3: 用export导出【这是hive的关键字】【该语句尽可能用 hive -e "sql" 去执行。不要用hue执行】
export table tmp_zfw.tmp_111 to '/home/zfw/20230220/test1';
```

## hive参数 {id="hive_2"}

```SQL
-- 强制使用MapReduce
set hive.fetch.task.conversion=none;

-- 关闭mapjoin
set hive.auto.convert.join=false;

-- 忽略锁
set hive.txn.manager = org.apache.hadoop.hive.ql.lockmgr.DummyTxnManager;
set hive.support.concurrency=false;

-- UDF操作
list jars;
delete jar .../xx.jar;
drop function default.xxxfunc;
create function default.xxxfunc as 'cn.xxx.XxxFunc' using jar 'hdfs:///upload/udf/xxx.jar';
create function ip_between as 'cn.das_security.sdp.udf.IpBetweenStartEnd' using jar 'hdfs://shuyan/user/lab01/jar/udf/lab01-sdp-hive-udfs-0.0.20.jar';

reload default.xxxfunc;

-- 设置执行器、线程、内存
SET spark.executor.instances=6;
SET spark.executor.cores=8;
SET spark.executor.memory=4g;


-- 空指针
set hive.auto.convert.join=false;
set hive.ignore.mapjoin.hint=false;

-- 报错
-- Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: 
-- DeserializeRead detail: Reading byte[] of length 265188 at start offset 0 for length 181058 to read 14 fields with types [string, array<string>, array<string>, array<string>, array<string>, bigint, array<string>, int, int, int, int, bigint, string, string].  Read field #6 at field start position 131 for field length 180893
-- 	at org.apache.hadoop.hive.ql.exec.vector.VectorMapOperator.process(VectorMapOperator.java:928)
-- 	... 19 more
-- Caused by: java.lang.ArrayIndexOutOfBoundsException
set hive.vectorized.execution.enabled=false;


-- 报错
-- Job aborted due to stage failure: Task 1 in stage 1.0 failed 4 times, most recent failure: Lost task 1.3 in stage 1.0 (TID 15, threat203, executor 1): java.lang.RuntimeException: Map operator initialization failed: org.apache.hadoop.hive.ql.metadata.HiveException: Unexpected column vector type LIST
set hive.vectorized.execution.enabled=false;



-- 报错
-- Error: Error while compiling statement: FAILED: SemanticException 20:84 AS clause has an invalid number of aliases. Error encountered near token 'view_ip' (state=42000,code=40000)
-- 自定义函数建的有问题，重建一下就好了

-- 报错
-- Caused by: java.lang.ClassCastException: org.apache.hadoop.hive.ql.exec.persistence.HashMapWrapper cannot be cast to org.apache.hadoop.hive.ql.exec.persistence.MapJoinTableContainerDirectAccess
set hive.mapjoin.optimized.hashtable=false;


 ROW FORMAT SERDE  
  'org.apache.hive.hcatalog.data.JsonSerDe' ;
  
-- 报错 (Container killed on request. Exit code is 143)
set mapreduce.map.memory.mb=4096
set mapreduce.reduce.memory.mb=4096
set yarn.nodemanager.vmem-pmem-ratio=4.2

set hive.auto.convert.join = true;


-- 强制提交到大数据平台
-- 默认是 more，老版本 hive 默认是 minimal，该属性修改为 more 以后，在全局查找、字段查找、limit 查找等都不走 MapReduce。
set hive.fetch.task.conversion=none;

-- 报错【该错误是hive单节点的执行的一个bug，绕过即可】
-- Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: java.io.IOException: java.io.IOException: error iterating
--     at org.apache.hadoop.hive.ql.exec.tez.MapRecordSource.pushRecord(MapRecordSource.java:80)
--     at org.apache.hadoop.hive.ql.exec.tez.MapRecordProcessor.run(MapRecordProcessor.java:426)
set hive.fetch.task.conversion=none;
```

## hive函数 {id="hive_3"}

```SQL
函数相关脚本参考：
-- UDF操作
list jars;
delete jar .../xx.jar;
drop function default.xxxfunc;
create function default.xxxfunc as 'cn.xxx.XxxFunc' using jar 'hdfs:///upload/udf/xxx.jar';
reload default.xxxfunc;
```

## 文件导入hive表

##### 1. 服务器文件

```Shell
# 1、服务器文件，每行一条记录，属性之间用逗号分割【也可以是其他分割符】，将所有文件放到同一个目录下

```

##### 2. 创建hive表

```SQL
CREATE TABLE tmp_zfw.tmp_test(
    col1 string,
    col2 int,
    col3 double
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
COLLECTION ITEMS TERMINATED BY '-'
MAP KEYS TERMINATED BY ':';
```

##### 3. 数据导入hive表

```SQL
-- 支持星号的通配符，也支持绝对路径【服务器路径】
LOAD DATA LOCAL INPATH '${file_dir}/*' OVERWRITE TABLE tmp_zfw.tmp_test;


-- 支持星号的通配符，也支持绝对路径【HDFS路径】
LOAD DATA INPATH '${file_dir}/*' OVERWRITE TABLE tmp_zfw.tmp_test;
```

## hive常见报错 {id="hive_4"}

##### 1. 空指针 {id="1_1"}

```SQL

-- 该SQL执行会报空指针
SELECT *
FROM ods_log.dbapp_sdc_threat_intelligence_api_log_dt
WHERE dt = '20231221'
  and pdt_name = 'sdb_portal_site'
  and array_contains(threat_type, 'WhiteList')
  and query_ip in (SELECT query_ip
                   FROM (SELECT query_ip, count(1) as num
                         FROM ods_log.dbapp_sdc_threat_intelligence_api_log_dt
                         WHERE dt = '20231221'
                           and pdt_name = 'sdb_portal_site'
                         group by query_ip
                         order by num desc) tab_1)
                         
-- 添加参数能解决                 
set hive.auto.convert.join=false;
set hive.ignore.mapjoin.hint=false;

        
```