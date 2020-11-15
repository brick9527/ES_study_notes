# 聚合查询

ES的聚合查询和MySQL的聚合查询类型，ES的聚合查询相比MySQL要强大的多，ES提供的统计数据的方式多种多样。

```json
# ES聚合查询的RESTful语法
POST /index/type/_search
{
  "aggs": {
    "名字（推荐agg）": {
      "AGG_TYPE": {
        "field": "VALUE"
      }
    }
  }
}
```

## 去重计数查询

去重计数，即`Cardinality`, 第一步先将返回的文档中的一个指定的field进行去重，统计一共有多少条

```json
# 去重计数查询 “北京”、“上海”、“武汉”、“山西”
POST /sms-logs-index/sms-log-type/_search
{
  "aggs": {
    "agg": { # 之后查询出来的内容也在一个名为"agg"的属性下
      "cardinality": { # 使用cardinality（去重计数）查询
        "filed": "province"
      }
    }
  }
}

# 查询出来的结果
{
  "aggregations": {
    "agg": {
      "value": 4
    }
  }
}
```

## 范围统计

统计一定范围内出现的文档个数，比如，针对某一个Field的值在 0~100,100~200,200~-300之间文档出现的个数分别是多少。

范围统计可以针对普通的数值，针对时间类型，针对ip类型都可以做相应的统计。

- 数值：`range`
- 日期：`date_range`
- ip：`ip_range`

### 数值方式范围统计

```json
POST /sms-logs-index/sms-log-type/_search
{
  "aggs": {
    "agg": {
      "range": {
        "field": "fee",
        "ranges": [
          {
            "to": 5 # <5
          },
          {
            "from": 5, # >=5
            "to": 10 # < 10
          },
          {
            "from": 10 # >=10
          }
        ]
      }
    }
  }
}
```

### 时间范围统计

```json
POST /sms-logs-index/sms-log-type/_search
{
  "aggs": {
    "agg": {
      "date_range": {
        "field": "createDate",
        "format": "yyyy",
        "ranges": [
          {
            "to": 2000
          },
          {
            "from": 2000
          }
        ]
      }
    }
  }
}
```

### ip统计方式

```json
POST /sms-logs-index/sms-log-type/_search
{
  "aggs": {
    "agg": {
      "ip_range": {
        "field": "ip",
        "ranges": [
          {
            "to": "10.126.2.9"
          },
          {
            "from": "10.126.2.9"
          }
        ]
      }
    }
  }
}
```

## 统计聚合查询

他可以帮你查询指定Field的最大值，最小值，平均值，平方和。。。。

使用类型：`extended_stats`

```json
# 统计聚合查询
POST /sms-logs-index/sms-log-type/_search
{
  "aggs": {
    "agg": {
      "extended_stats": {
        "field": "fee"
      }
    }
  }
}
```