# work-Hadoop

### 大数据hdfs命令

```shell
### 文件重平衡【节点间的平衡】
./start-balancer.sh -threshold 3

### 查看重平衡的进程
ps -ef | grep balancer

### 查看重平衡的日志
### 上一步的进程中能看到日志的输出位置

### 切换hdfs的master节点。将master节点切换到nn2
hdfs haadmin -transitionToActive nn2 --forcemanual
hdfs haadmin -transitionToActive --forcemanual --forceactive nn2

### ### 文件重平衡【节点内的磁盘间平衡】
## 生成平衡计划
hdfs  diskbalancer -plan threat204

## 执行平衡计划【后面的文件是前一步生成的返回文件】
hdfs diskbalancer -execute /system/diskbalancer/2023-May-10-00-10-25/threat204.plan.json

## 查看执行进度【PLAN_UNDER_PROGRESS：正在执行，PLAN_DONE：执行完成】
hdfs diskbalancer -query threat204




## 查看各节点的磁盘占用情况
hdfs dfsadmin -report


## 对物理磁盘配额
hdfs dfsadmin -setSpaceQuota 1000G /home
```

### 大数据配置

```xml
<!-- 针对单个节点配置hdfs-site.xml，表示当前节点，DataNode需要预留的磁盘空间大小，单位是 B -->
<!-- 配置后需要重启datanode-->
<!-- ./sbin/hadoop-daemons.sh stop datanode -->
<!-- ./sbin/hadoop-daemons.sh start datanode -->
<property>
    <name>dfs.datanode.du.reserved</name>
    <value>21474836480</value>
</property>


```

### 大数据增加节点

```shell
#1、设置免密【需要和各个节点互通】

#2、配置java环境【java版本需要一致】

#3、拷贝hadoop【从原有环境copy hadoop的$HADOOP_HOME文件夹及其子文件 到新节点】

#4、修改各个节点的 $HADOOP_HOME/etc/hadoop/workers 文件，将新节点的主机名称添加到文件中，分发到其他节点【方便下一次启动的时候，可以一建重启】

#5、若有需要，可以调整新节点的内存、CPU等资源占用情况【yarn-site.xml】
#6、若有需要，可以调整新节点的数据存储目录情况和占用磁盘资源情况【hdfs-site.xml】

#7、启动新节点的yarn和hdfs
cd $HADOOP_HOME/sbin
./start-yarn.sh 启动ResourceManager、NodeManager

#8、观察hdfs和yarn，新节点是否上线

#9、如果hdfs没有正常上线，需要执行下面创建目录的命令
mkdir -p /home/export/tmp/hadoop/hdfs


```

### 统计文件个数
```Shell
hdfs dfs -ls -R /user/hive/warehouse/intelligence_pre.db/dwd_threat_intelligence_evidence_data_dt/dt=20240508 | grep -v '^d' | wc -l
```

