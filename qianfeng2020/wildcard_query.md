# wildcard查询

通配符查询，和MySQL中的`like`是一个套路，可以在查询时，在字符串中指定通配符*和占位符?

*通配所有字符，?通配一个字符

```json
# wildcard查询
POST /sms-logs-index/sms-log-type/_search
{
  "query": {
    "wildcard": {
      "cropName": {
        "value": "中国*" # 可以使用*和?指定通配符和占位符
      }
    }
  }
}
```