# range查询

范围查询，只针对数值类型，对某一个Field进行大于或者小于的范围指定

- gt： 大于
- gte：大于等于
- lt：小于
- lte：小于等于

```json
# range查询
POST /sms-logs-index/sms-log-type/_search
{
  "query": {
    "range": {
      "fee": {
        "gte": 5,
        "lte": 10
      }
    }
  }
}
```