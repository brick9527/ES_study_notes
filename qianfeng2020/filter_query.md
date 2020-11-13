# filter查询

- query：根据你的查询条件，去计算文档的匹配度得到一个分数，并且根据分数进行排序，不会做缓存的。
- filter：根据你的查询条件去查询文档，不去计算分数，不排序，而且filter会对经 常被过滤的数据进行缓存。

```json
# filter查询
POST /sms-logs-index/sms-log-type/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "corpName": "盒马鲜生"
          }
        },
        {
          "range": {
            "fee": {
              "lte": 5
            }
          }
        }
      ]
    }
  }
}
```