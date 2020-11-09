# prefix查询

前缀查询，可以通过一个关键字去指定一 个Field的值的前缀，从而查询到指定的文档。

针对`keyword`类型也可以，但是match却不能。

```json
POST /sms-logs-index/sms-logs-type/_search
{
  "query": {
    "prefix": {
      "corpName": {
        "value": "途虎" # 查询corpName值以"途虎"开头的结果
      }
    }
  }
}
```