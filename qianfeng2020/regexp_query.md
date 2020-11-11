# regexp查询

正则查询，通过你编写的正则表达式去匹配内容。

`prefix`，`fuzzy`，`wildcard`和`regexp`查询效率相对比较低，要求效率比较高时，应避免使用。

```json
# regexp查询
POST /sms-logs-index/sms-log-type/_search
{
  "query": {
    "regexp": {
      "mobile": "180[0-9]{8}" # 正则表达式
    }
  }
}
```