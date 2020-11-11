# scroll深分页

ES对from+size是有限制的，from和size二者之和不能超过1W。

原理：

from+size在ES查询数据的方式：

- 第一步，现将用户指定的关键进行分词。
- 第二步，将词汇去分词库中进行检索，得到多个文档的id。
- 第三步，去各个分片中去拉取指定的数据。（耗时较长）
- 第四步，将数据根据score进行排序。（耗时较长）
- 第五步，根据from的值，将查询到的数据舍弃一部分。
- 第六步，返回结果。

scroll+size在ES查询数据的方式：

- 第一步，先将用户指定的关键进行分词。
- 第二步，将词汇去分词库中进行检索，得到多个文档的id。
- 第三步，将文档的id存放在一个ES的上下文中。
- 第四步，根据你指定的size去ES中检索指定的数据，拿完数据的文档id,会从上下文中移除。
- 第五步，如果需要下一页数据，直接去ES的上下文中，找后续内容。
- 第六步，循环第四步和第五步

scroll查询方式，不适合做实时的查询

```json
# 执行scroll查询，返回第一页数据，并且将文档id信息存放在ES上下文中，指定生存时间为1m(scroll=1m)
POST /sms-logs-index/sms-log-type/_search?scroll=1m
{
  "query": {
    "match_all": {}
  },
  "size": 2,
  "sort": [
    {
      "fee": {
        "order": "desc"
      }
    }
  ]
}

# 根据scroll查询下一页数据
POST /_search/scroll
{
  "scroll_id": <上一步返回的scroll_id>,
  "scroll": "1m" # 设置scroll在上下文中存活时间
}

# 删除scroll在ES上下文中的数据
DELETE /_search/scroll/<scroll_id>
```