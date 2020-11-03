# 创建索引并指定结构

```json
# 创建索引，指定数据结构
PUT  /book
{
  "settings": {
    # 分片数，默认：5
    "number_of_shards": 5,
    # 备份数，默认：1
    "number_of_replicas": 1
  },
  # 指定数据结构
  "mappings": {
    # 声明Type名称
    "novel": {
      # 各字段类型
      "properties": {
        "name": {
          "type": "text",
          "analyzer": "ik_max_word", # 指定分词器
          "index": true, # 指定当前Field可以被作为查询条件
          "store": false # 是否需要额外存储
        },
        "author": {
          "type": "keyword"
        },
        "count": {
          "type": "long"
        },
        "on-sale": {
          "type": "date",
          # 时间类型的格式化方法
          "format": "yyyy-MM-dd HH:mm:ss || yyyy-MM-dd || epoch_millis" 
        },
        "descr": {
          "type": "text",
          "analyzer": "ik_max_word",
        }
      }
    }
  }
}
```