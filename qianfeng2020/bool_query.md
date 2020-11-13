# 复合查询

## bool查询

复合过滤器，将你的多个查询条件，以定的逻辑组合在一起。

- must：所有的条件，用must组合在一起，表示And的意思
- must_not：将must_ _not中的条件，全部都不能匹配，表示Not的意思
- should：所有的条件，用should组合在一 起，表示Or的意思

```json
# 查询省份为武汉或者北京
# 运营商不是联通
# smsContent中包含“中国”和“平安”
# bool查询
POST /sms-logs-index/sms-log-type/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "term": {
            "province": {
              "value": "北京"
            }
          }
        },
        {
          "term": {
            "province": {
              "value": "武汉"
            }
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "operatorId": {
              "value": "2"
            }
          }
        }
      ],
      "must": [
        {
          "match": {
            "smsContent": "中国"
          }
        },
        {
          "match": {
            "smsContent": "平安"
          }
        }
      ]
    }
  }
}
```

## boosting查询

boosting查询可以帮助我们去影响查询后的score。

- positive：只有匹配上positive的查询的内容，才会被放到返回的结果集中。
- negative：如果匹配上positive并且也匹配上了negative,就可以降低这样的文档score。
- negative_boost：指定系数，必须小于1.0

关于查询时，分数是如何计算的:

- 搜索的关键字在文档中出现的频次越高，分数就越高
- 指定的文档内容越短，分数就越高
- 我们在搜索时，指定的关键字也会被分词，这个被分词的内容，被分词库匹配的个数越多，分数越高

```json
# boosting查询 “收货安装”
POST /sms-logs-index/sms-log-type/_search
{
  "query": {
    "boosting": {
      "positive": {
        "match": {
          "smsContent": "收货安装"
        }
      },
      "negative": {
        "match": {
          "smsContent": "王五"
        }
      },
      "negative_boost": 0.5
    }
  }
}
```