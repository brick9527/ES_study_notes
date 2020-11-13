# delete-by-query

根据term，match等查询方式去删除大量的文档

**Ps:如果你需要删除的内容是index下的大部分数据，推荐创建一个全新的index,将保留的文档内容，添加到全新的索引**

```json
#delete-by-query
POST /sms-logs-index/sms-log-type/_delete_by_query
{
  "query": {
    "range": {
      "fee": {
        "lt": 4
      }
    }
  }
}
```