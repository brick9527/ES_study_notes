# 第十七课 Query String&Simple Query String查询

# 一、Query String Query

- 类似URI Query

```sh
POST users/_search
{
  "query": {
    "query_string": {
      "default_field": "name",
      "query": "Ruan AND Yiming"
    }
  }
}


POST users/_search
{
  "query": {
    "query_string": {
      "fields":["name","about"],
      "query": "(Ruan AND Yiming) OR (Java AND Elasticsearch)"
    }
  }
}
```

# Simple Query String Query

- 类似Query String，但是会忽略错误的语法，同时只支持部分查询语法
- 不支持`AND`,`OR`,`NOT`，会当做字符串处理
- Term之前默认的关系是`OR`，可以指定`operator`
- 支持部分逻辑
  - `+`替代AND
  - `|`替代OR
  - `-`替代NOT

```sh
#Simple Query 默认的operator是 Or
POST users/_search
{
  "query": {
    "simple_query_string": {
      "query": "Ruan AND Yiming",
      "fields": ["name"]
    }
  }
}


POST users/_search
{
  "query": {
    "simple_query_string": {
      "query": "Ruan Yiming",
      "fields": ["name"],
      "default_operator": "AND"
    }
  }
}

```