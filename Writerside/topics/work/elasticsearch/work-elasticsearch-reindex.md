# work-elasticsearch-reindex

### 本地reindex

```SQL
POST _reindex?wait_for_completion=true
{
  "source": {
    "index": "person"
  },
  "dest": {
    "index": "people"
  }
}
```

### 远程reindex {id="reindex_1"}

1. elasticsearch.yml 加入配置
    ```Shell
    # 不加会报错 
    # "reason" : "[xxxxxxx:9200] not whitelisted in reindex.remote.whitelisted"
    reindex.remote.whitelist: [sorce:9200]
    ```

2. 对SSL，要加入源的证书
    ```Shell
    # 不加可能会报以下错 
    # unable to find valid certification path to requested target

    reindex.ssl.certificate_authorities: certs/elasticsearch-ca.pem
    reindex.ssl.verification_mode: certificate
    ```
3. 开始reindex
   ```Shell
   POST _reindex?wait_for_completion=false
   {
     "source": {
       "remote": {
         "host": "https://win88.inno.com:9200",
         "username": "elastic",
         "password": "xxxxxxx"
       },
       "index": "student"
     },
     "dest": {
       "index": "student"
     }
   }
   ```

4. 查看结果成功
   ```Shell
   get student/_search
   ```



