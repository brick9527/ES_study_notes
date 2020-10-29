# 基本Rest命令说明

|method|url地址|描述|
|-|-|-|
|PUT|localhost:9200/索引名称/类型名称/文档ID|创建文档（指定文档ID）|
|POST|localhost:9200/索引名称/类型名称|创建文档（随机文档ID）|
|POST|localhost:9200/索引名称/类型名称/文档ID/_update|修改文档|
|DELETE|localhost:9200/索引名称/类型名称/文档ID|删除文档|
|GET|localhost:9200/索引名称/类型名称/文档ID|查询文档通过文档ID|
|POST|localhost:9200/索引名称/类型名称/_search|查询所有数据|

**注意**

PUT修改会把全部文档都修改了，POST修改能做到只修改要修改的那部分数据（其他的保持不变）

# 搜索

### 简单的搜索

```sh
GET /kuangsheng/user/1
```

### 带条件的搜索

```sh
GET /kuangshenshuo/user/_search?q=name:张三
```

# 复杂搜素

查询的参数体，使用JSON构建

### 示例1

```sh
GET /student/user/_search
{
  "query": {
    "match": {
      "name": "张三"
    }
  }
}
```

查询出来的hits：

- 索引和文档信息
- 查询的结果总数
- 查询出来的具体的文档
- 可以通过score判断谁更加符合结果 

### 示例2

结果的过滤，只查询`name`和`age`字段

```sh
GET /student/user/_search
{
  "query": {
    "match": {
      "name": "张三"
    }
  },
  "_source": ["name", "age"]
}
```

### 示例3

排序

按照`age`字段降序(desc)排序
```sh
GET /student/user/_search
{
  "query": {
    "match": {
      "name": "张三"
    }
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ]
}
```

### 分页

```sh
GET /student/user/_search
{
  "query": {
    "match": {
      "name": "张三"
    }
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ],
  "from": 0,
  "size": 2
}
```

- from: 从第几条开始
- size：返回多少条数据

数据索引下标还是从0开始

### 布尔值查询

#### must

and操作，所有条件都要符合

```sh
GET /student/user/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "张三"
          }
        },
        {
          "match": {
            "age": 3
          }
        }
      ]
    }
  }
}
```

#### should

or操作，满足一个即可

```sh
GET /student/user/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "name": "张三"
          }
        },
        {
          "match": {
            "age": 3
          }
        }
      ]
    }
  }
}
```

#### must_not

not操作

```sh
GET /student/user/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "match": {
            "name": "张三"
          }
        },
        {
          "match": {
            "age": 3
          }
        }
      ]
    }
  }
}
```

#### 过滤器

```sh
GET /student/user/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "张三"
          }
        },
        {
          "match": {
            "age": 3
          }
        }
      ],
      "filter": {
        "range": {
          "gte": 10,
          "lte": 20
        }
      }
    }
  }
}
```

- gt:>
- lt:<
- gte:>=
- lte:<=

#### 多条件查询

只要`tags`中包含`男`或者`技术`都会被查询出来。多个条件使用空格隔开。匹配数量越多，得分越高

```sh
GET /student/user/_search
{
  "query": {
    "match": {
      "tags": "男 技术"
    }
  }
}
```

#### 精确查询

term查询是直接通过倒排索引指定的词条进行精确查找。

关于分词：

- term：直接查询精确的值
- match：会使用分词器解析（先分析文档，然后再通过分析的文档进行查询）

两个类型：

- text
- keyword


没有被拆分，`你好hello world`

```sh
GET _analyze
{
  "analyzer": "keyword",
  "text": "你好hello world"
}
```

被拆分了，`你`，`好`，`hello`，`world`

```sh
GET _analyze
{
  "analyzer": "standard",
  "text": "你好hello world"
}
```

精确查询

```sh
GET /student/user/_search
{
  "query": {
    "term": {
      "name": "张"
    }
  }
}
```

keyword类型字段不会被分词器解析

多个值查询：

`22`和`33`都能被查询出来

```sh
GET /student/user/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "term": {
            "t1": "22"
          }
        },
        {
          "term": {
            "t1": "33"
          }
        }
      ]
    }
  }
}
```

高亮查询：

搜索的结果，将会在结果中用`<em></em>`标记、高亮起来。

```sh
GET /student/user/_search
{
  "query": {
    "match": {
      "name": "张三"
    }
  },
  "highlight": {
    "fields": {
      "name"
    }
  }
}
```

自定义搜索高亮条件：

设置默认的包裹，吧`<em></em>`改了

```sh
GET /student/user/_search
{
  "query": {
    "match": {
      "name": "张三"
    }
  },
  "highlight": {
    "pre_tags": "<p class='key' style='color:red'>",
    "post_tags": "</p>",
    "fields": {
      "name"
    }
  }
}
```