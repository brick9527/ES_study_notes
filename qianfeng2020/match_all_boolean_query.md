# match和布尔查询

## match查询

指定一个Field作为筛选条件

会对查询关键字进行分词，然后去分词库进行匹配

```json
# match查询
POST /sms-logs-index/sms-logs-type/_search
{
  "query": {
    "match": {
      "smsContent": "收货安装"
    }
  }
}
```

## match布尔查询

基于一个Field匹配的内容，采用`and`或者`or`的方式连接

```json
# 布尔match查询
POST /sms-logs-index/sms-logs-type/_search
{
  "query": {
    "match": {
      "smsContent": {
        "query": "中国 健康",
        "operator": "and" #查询内容既包含“中国”也包含“健康”的
      }
    }
  }
}

# 布尔match查询
POST /sms-logs-index/sms-logs-type/_search
{
  "query": {
    "match": {
      "smsContent": {
        "query": "中国 健康",
        "operator": "or" #查询内容包含“中国”或“健康”的
      }
    }
  }
}
```