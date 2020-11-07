# term & terms查询

## term查询

term的查询是代表完全匹配，搜索之前不会对你搜索的关键字进行分词，对你的关键字去文档分词库中去匹配内容。

```json
# term 查询
POST /sms-logs-index/sms-logs-type/_search
{
  "from": 0,
  "size": 5,
  "query": {
    "term": {
      "province": "北京"
    }
  }
}
```

## terms查询

terms和term的查询机制是一样的，都不会将指定的查询关键字进行分词，直接去分词库中匹配，找到相应文档内容。

terms是在针对一个字段包含多个值的时候使用。

- term: where province = '北京'
- terms where province in ('北京', '山西' ...)

```json
# terms 查询
POST /sms-logs-index/sms-logs-type/_search
{
  "query": {
    "terms": {
      "province": [ "北京", "山西" ]
    }
  }
}
```

term & terms查询并不只是针对`keyword`类型，也可以对`text`类型使用，但是**查询条件不会分词**，也就可能匹配不到分词库中完整的句子。