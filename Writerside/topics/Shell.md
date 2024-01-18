# Shell

## 操作防火墙
```shell
# 开放端口
firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.23.30.7" port port="8042" protocol="tcp" accept' && firewall-cmd --reload && firewall-cmd --list-all

# 开放IP
firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.23.30.7"   accept' && firewall-cmd --reload && firewall-cmd --list-all

firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.11.47.212"   accept' && firewall-cmd --reload && firewall-cmd --list-all

# 开放CIDR
firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.50.25.0/24" accept'  && firewall-cmd --reload && firewall-cmd --list-all

```

## curl 下载文件

```shell
curl  --request GET  --remote-name --remote-header-name  http://10.43.82.210:8810/v3/intelligence/inbound/db/hvv

curl --request GET --header "sdc-token: 2429bd8846a8dca1a35462eafb0a140ced60e58d" --remote-name --remote-header-name https://tapi.dbappsecurity.com.cn/daas/lab01/v3/intelligence/inbound/db/block
```

## es数据dump

```shell
elasticdump --input http://10.50.25.205:9200/dbapp_sdc_threat_intelligence_hvv_2023_index-2023051210 --output http://10.50.25.210:9200/dbapp_sdc_threat_intelligence_hvv_2023_index-2023051210 --limit 10000

elasticdump --input http://10.50.25.210:9200/dbapp_sdc_threat_intelligence_ip_visualization_top_index-20231207 --output http://10.50.25.205:9200/dbapp_sdc_threat_intelligence_ip_visualization_top_index-20231207 --limit 10000

elasticdump --input http://10.50.25.210:9200/dbapp_sdc_threat_intelligence_ip_visualization_top_index-20231205 --output http://10.50.25.205:9200/dbapp_sdc_threat_intelligence_ip_visualization_top_index-20231205 --limit 10000 --searchBody={\"query\":{\"term\":{\"ip_range_list\":{\"value\":\"192.155.88.231\"}}}}

elasticdump \
  --input=http://10.50.25.205:9200/dbapp_sdc_threat_intelligence_agg_index-1130110743 \
  --limit 10000 \
  --output=data.json \
  --type=data \
  --size 10000 \
  --json-lines
```

```shell
elasticdump \
  --input=http://10.50.25.205:9200/dbapp_sdc_threat_intelligence_agg_index \
  --limit 10000 \
  --output=data.json \
  --type=data \
  --size 100000 \
  --json-lines
#还有几个比较重要的参数：
# limit 限制每次批量的文档数：--limit 2000
# size 用来限制同步的文档总量：--size 100000
# searchBody 通过查询来限制数据范围:--searchBody={\"query\":{\"terms\":{\"ioc\":[\"27.157.129.180\",\"27.221.6.108\"]}}}
```


## 在文件夹下搜索内容

```shell
# 在文件夹及其子文件夹下搜索内容
grep -r -e '"d{"ip"' ./
```

## 系统指标

```shell
# （CPU、内存、磁盘读写速度等）查看
# 可以用  **sysstat** 工具
```


## 查看目录大小

```shell
du  -sh   目录名称
```



## linux远程挂载

1. 服务端

    1. 检查nfs服务

       ```shell
       rpm -qa|grep nfs
       rpm -qa|grep rpcbind
       ```

    2. 安装nfs

       ```shell
       yum -y install nfs-utils rpcbind
       ```

    3. 设置开机自动启动服务

       ```shell
       chkconfig nfs on
       chkconfig rpcbind on
       ```

    4. 创建共享目录:

       ```shell
       mkdir /data/nfs-share
       chmod -R 777 /data/nfs-share
       ```

    5. 配置exports文件权限

       ```shell
       vim /etc/exports
       加入：
       /data/nfs-share 192.168.1.1(rw)
       ```

    6. 刷新配置:

       ```shell
       exportfs -a
       ```

2. 客户端

    1. 创建共享目录

       ```shell
       #这个目录主要是去 共享服务端分享的那个目录，同步那个文件
       mkdir  /data/share-file   
       ```

    2. 挂载目录:

       ```shell
       #  这个是挂载
       mount 192.168.1.1:/data/nfs-share /data/share-file  
       
       
       # 这是卸载
       umount /data/share-file  
       ```

## ssh代理

```shell
端口转发至本地：
#通过svr的ssh隧道，将svr可以访问的rhost的rport 转发至 本机lhost，监听lhost为lport(在执行命令的机器上监听端口，而非在svr上监听端口)
ssh -L lhost:lport:rhost:rport  root@svr

#可选参数：
-f 后台启用
-N 不打开远程shell，处于等待状态
-g 启用网关功能

端口转发至远端
#让svr监听svrport，并且将svrport收到的请求转发给rhost的rport

ssh -R svrport:rhost:rport  root@svr

#可选参数：
-f 后台启用
-N 不打开远程shell，处于等待状态
-g 启用网关功
socks代理服务
#监听本机的1080端口，作为socks服务，将收到的请求由sshserver进行转发
ssh -D 1080 root@sshserver

ssh -fND 10.50.25.68:1081 root@172.16.5.135
ssh -fND 10.50.25.68:1080 root@127.0.0.1
```



## tinyproxy 代理

[代理配置详解](https://developer.aliyun.com/article/1009640)

tinyproxy版本1.8.3不支持账号密码验证，而新版本1.10.0支持

1、如果不需要鉴权，可以直接通过yum安装1.8版本

2、需要鉴权则要使用1.10版本，yum安装的最新版是1.8，只能通过源码安装

### yum安装1.8.3

```shell
# 安装
$ yum install tinyproxy

$ tinyproxy -v
tinyproxy 1.8.3

# 启动 start|stop|status|restart
$ service tinyproxy start

# 卸载
$ yum erase tinyproxy
```



### 编译安装1.10.0

[下载安装包页面](https://github.com/tinyproxy/tinyproxy/releases)

找到最新版本 Version 1.10.0 （2020-05-20），看到新版本的介绍，已经增加了验证

1. 安装最新版

   ```shell
   wget https://github.com/tinyproxy/tinyproxy/releases/download/1.10.0/tinyproxy-1.10.0.tar.gz
   
   tar -zxvf tinyproxy-1.10.0.tar.gz
   
   cd tinyproxy-1.10.0
   
   # 编译安装【需要gcc的支持。没有的话可以执行 `yum -y install gcc` 】
   ./configure && make && make install
   
   $ which tinyproxy
   /usr/local/bin/tinyproxy
   
   $ tinyproxy -v
   tinyproxy 1.10.0
   
   # 如果发现tinyproxy的版本没有变化，则删除文件重新进行编译安装
   ```

2. 修改配置

   ```shell
   vim /etc/tinyproxy/tinyproxy.conf
   
   
       # 注释掉这行，允许所有ip访问
       # Allow 127.0.0.1
   
       # 权限校验
       BasicAuth user 123456
   ```

   如果配置文件不存在，则搜索一下配置文件路径

   ```shell
   $ find / -name tinyproxy.conf
   
   # 拷贝一份配置文件
   $ cp tinyproxy.conf /etc/tinyproxy/tinyproxy.conf
   ```

3. 启动

   ```shell
   # 启动（不采用后台启动，方便调试）
   $ tinyproxy -d -c /etc/tinyproxy/tinyproxy.conf
   
   # 指定配置文件启动（后台启动）
   $ tinyproxy -c /etc/tinyproxy/tinyproxy.conf
   
   # 杀掉进程
   $ ps -ef|grep tinyproxy|grep -v grep|awk &#39;{print &#34;kill -9 &#34;$2}'|sh
   ```

4. 测试

   ```shell
   # 不加验证参数不会正常返回
   $ curl -x http://127.0.0.1:8888 www.baidu.com
   Proxy Authentication Required
   
   # 正常返回
   $ curl -x http://user:123456@127.0.0.1:8888 www.baidu.com
   ```

### 遇到的问题

    + 之前通过yum安装过1.8版本，又通过编译安装了1.10版本，版本号没有变化

      ```shell
      解决：
      将tinyproxy彻底删除后重新编译安装
      
      $ find / -name tinyproxy
      
      ```

    + 配置文件不生效

      ```shell
      看下是不是有多份配置文件，最好通过-c指定配置文件
      
      
      ```

## 查看mysql的binlog日志
```Shell
# 查看 binlog 日志相关参数
show variables like  'binlog%';

# 设置对应参数
set global binlog_expire_logs_seconds = 864000;

# 参数设置好，需要立马生效的话，可以刷新一下
flush logs


# mysql-bin.000008 是对应的binlog日志文件
# 该命令能够按照操作频率降序排列
mysqlbinlog --no-defaults --base64-output=decode-rows -v -v mysql-bin.000008 |awk '/###/{if($0~/UPDATE|INSERT|DELETE/)count[$2" "$NF]++}END{for(i in count)print i,"\t",count[i]}'|column -t|sort -k3nr|more


```


## 查看mysql的锁表情况
```sql
# 显示表的使用情况
show OPEN TABLES where In_use > 0;

```


## ftp服务器搭建
[阿里教程](https://help.aliyun.com/document_detail/60152.html?spm=5176.21213303.J_6704733920.8.7b2f53c9y3JtYO&scm=20140722.S_help%40%40%E6%96%87%E6%A1%A3%40%4060152._.ID_help%40%40%E6%96%87%E6%A1%A3%40%4060152-RL_%E6%90%AD%E5%BB%BAftp%E6%9C%8D%E5%8A%A1%E5%99%A8-LOC_main-OR_ser-V_2-RK_rerank-P0_0)



## 各种请求挂代理

```shell
# wget 
wget -e use_proxy=on -e "HTTPS_PROXY=http://root:8.208.80.213fKjcd3-6FD6-4C31-9ZZG3-fNM4345f62ea@8.208.80.213:9999" -O 2023-02-22-1677024443-rdns.json.gz  https://f002.backblazeb2.com/file/rapid7-opendata/sonar.rdns_v2/2023-02-22-1677024443-rdns.json.gz?Authorization=3_20230301060301_94e9f4273ad6f53ea166027e_77c2ccb91d599f75c1a22868a0f6f66e669c466d_002_20230302060301_0048_dnld  --no-check-certificate

# curl
curl -x http://root:8.208.80.213fKjcd3-6FD6-4C31-9ZZG3-fNM4345f62ea@8.208.80.213:9999 https://www.baidu.com

```



## k8s操作

```shell
# 查看命名空间
kubectl get ns | grep operate
# 查看deployments
kubectl -n das-sdc-operate get deployments | grep inbound
# 编辑部署配置，比如修改版本号，编辑操作跟vim几乎一样
kubectl -n das-sdc-operate edit deployments sdp-operation-web
# 查看部署的容器
kubectl -n das-sdc-operate get pods | grep inbound
# 查看日志
kubectl -n das-sdc-operate logs -f sdp-produce-inbound-7df77b85df-knk8g
# 进入容器执行命令
kubectl -n das-sdc-operate exec -it spider-flow-7bf8f8dcf6-9rpht sh

# 查看容器占用内存情况
kubectl top pods -A --sum=true | grep sdp-produce-inbound
```
