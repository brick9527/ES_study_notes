# fuzzy查询

模糊查询，我们输入字符的大概，ES就可以去根据输入的内容大概去匹配一 下结果。

```json
POST /sms-logs-index/sms-logs-type/_search
{
  "query": {
    "fuzzy": {
      "corpName": {
        "value": "盒马先生",
        "prefix_length": 2 # 指定前面几个（2）个字符不允许出现错误
      }
    }
  }
}
```