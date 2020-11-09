# multiMatch查询

match针对一个field做检索，multi_match针对多个field做检索，多个field对应一个text。

```json
POST /sms-logs-index/sms-logs-type/_search
{
  "query": {
    "multi_search": {
      "query": "北京", # 指定搜索关键字
      "fields": [ "province", "smsContent" ] # 指定各个匹配的field
    }
  }
}
```