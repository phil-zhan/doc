# work-doc-threat_analysis

## 基于日志的威胁分析安装教程

### 1. 安装前的准备
1. 检查 jdk-17.0.11 目录是否存在
2. 检查 neo4j-community-5.20.0-unix.tar.gz 文件是否存在
3. 检查 threat-analysis-0.0.1-SNAPSHOT.jar 文件是否存在

### 2. 安装neo4j
1. 新建 Dockerfile 文件
    ```
    # 使用基础镜像
    FROM docker.das-security.cn/centos:7.9.2009
    
    # 设置环境变量
    ENV JAVA_HOME=/usr/local/jdk-17.0.11
    ENV PATH=$JAVA_HOME/bin:$PATH
    
    # 复制文件到容器中
    COPY jdk-17.0.11 /usr/local/jdk-17.0.11
    
    # 创建工作目录并复制neo4j压缩包
    WORKDIR /app
    COPY neo4j-community-5.20.0-unix.tar.gz /app/neo4j-community-5.20.0-unix.tar.gz
    
    # 解压neo4j
    RUN tar -xzf neo4j-community-5.20.0-unix.tar.gz && \
        mv neo4j-community-5.20.0 /app/neo4j && \
        rm neo4j-community-5.20.0-unix.tar.gz
    
    # 设置neo4j的环境变量
    ENV NEO4J_HOME=/app/neo4j
    ENV PATH=$NEO4J_HOME/bin:$PATH
    
    # 暴露neo4j默认端口
    EXPOSE 7474 7687
    
    # 启动neo4j
    CMD ["neo4j", "console"]
    ```

2. 制作neo4j镜像
    ```shell
   docker build -t docker.das-security.cn/lylab/neo4j-community:5.20.0 .
    ```
3. 启动neo4j的docker容器
    ```shell
   docker run -p 7474:7474 -p 7687:7687 -v logs:/app/neo4j/logs -d --name neo4j-community docker.das-security.cn/lylab/neo4j-community:5.20.0
    ```

### 2. 部署分析服务
1. 新建 Dockerfile 文件
    ```
    # 使用基础镜像
    FROM docker.das-security.cn/centos:7.9.2009
   
    # 创建工作目录并复制neo4j压缩包
    WORKDIR /app 
   
    # 设置环境变量
    ENV JAVA_HOME=/usr/local/jdk-17.0.11
    ENV PATH=$JAVA_HOME/bin:$PATH
    
    # 复制文件到容器中
    COPY jdk-17.0.11 /usr/local/jdk-17.0.11

    # 将本地的 threat-analysis-0.0.1-SNAPSHOT.jar 文件拷贝到容器中
    COPY threat-analysis-0.0.1-SNAPSHOT.jar /app/threat-analysis-0.0.1-SNAPSHOT.jar
   
    # 暴露应用运行端口（根据实际需要修改）
    EXPOSE 8787
   
    # 启动容器时执行命令
    ENTRYPOINT ["java", "-jar", "/app/threat-analysis-0.0.1-SNAPSHOT.jar"]
    ```

2. 制作neo4j镜像
    ```shell
    docker build -t docker.das-security.cn/lylab/threat-analysis-prod:1.0.0 .
    ```
3. 启动neo4j的docker容器
    ```shell
    docker stop threat-analysis-prod && docker rm threat-analysis-prod

    docker run -p 8787:8787 -v threat-analysis-prod/logs:/app/logs -d --name threat-analysis-prod docker.das-security.cn/lylab/threat-analysis-prod:1.0.0
   
    echo "启动完成，服务地址是：http://<ip>:8787"
    ```


### 3. 配置智能体