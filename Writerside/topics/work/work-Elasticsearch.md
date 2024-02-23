# work-Elasticsearch

## ES复杂语句

```Shell
## 统计情报数量
GET dbapp_sdc_threat_intelligence_agg_index/_search
{
  "size": 0,
  "query": {
    "term": {
      "threat_types": {
        "value": "WhiteList"
      }
    }
  },
  "aggs": {
    "index_sum": {
      "sum": {
        "script": {
          "source": "doc['end'].value - doc['start'].value + 1"
        }
      }
    }
  }
}
```

## 常用索引

```Shell

### 聚合索引
GET dbapp_sdc_threat_intelligence_agg_index/_search

### 全量索引
GET dbapp_sdc_threat_intelligence_agg_all_index/_search

### 行为画像TOP
GET dbapp_sdc_threat_intelligence_ip_visualization_top_index/_search

### 行为画像TIME
GET dbapp_sdc_threat_intelligence_ip_visualization_time_index/_search

### 攻击画像
GET dbapp_sdc_threat_intelligence_ip_visualization_attack_index/_search
```
