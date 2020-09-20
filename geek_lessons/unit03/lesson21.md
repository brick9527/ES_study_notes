# 第二十一课 Index Template和Dynamic Template

# 什么是Index Template

- Index Template - 帮助你设定Mappings和Settings，并按照一定的规则，自动匹配到新创建的索引之上
  - 模板仅在一个索引被创建时，才会产生作用。修改模板不会影响已创建的索引
  - 你可以设定多个索引模板，这些设置会被“merge”在一起
  - 你可以指定“order”的数值，控制“merging”的过程

# Index Template的工作方式

- 当一个索引被新创建时
  - 首先应用ES默认的settings和mappings
  - 再应用order数值低的Index Template中的设定
  - 然后应用order数值高的Index Template中的设定，之前的设定会被覆盖
  - 应用创建索引时，用户所指定的Settings和Mappings，并覆盖之前模板中的设定

# Demo

- 创建2个Index Template
- 查看根据名字查看Template
- 查看所有的templates，_template/*
- 创建一个临时索引，查看replica和数据类型推断
- 将索引名字设置为能Index Template匹配时，查看所生成的Index的mappings和setting

```sh
#数字字符串被映射成text，日期字符串被映射成日期
PUT ttemplate/_doc/1
{
	"someNumber":"1",
	"someDate":"2019/01/01"
}
GET ttemplate/_mapping


#Create a default template
PUT _template/template_default
{
  "index_patterns": ["*"],
  "order" : 0,
  "version": 1,
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas":1
  }
}


PUT /_template/template_test
{
    "index_patterns" : ["test*"],
    "order" : 1,
    "settings" : {
    	"number_of_shards": 1,
        "number_of_replicas" : 2
    },
    "mappings" : {
    	"date_detection": false,
    	"numeric_detection": true
    }
}

#查看template信息
GET /_template/template_default
GET /_template/temp*


#写入新的数据，index以test开头
PUT testtemplate/_doc/1
{
	"someNumber":"1",
	"someDate":"2019/01/01"
}
GET testtemplate/_mapping
get testtemplate/_settings
```

# 什么是Dynamic Template

- 根据ES识别的数据类型，结合字段名称，来动态设定字段类型
  - 所有的字符串类型都设定成keyword，或者关闭keyword字段
  - is开头的字段都设置成boolean
  - long_开头的都设置成long类型

# Dynamic Template

- Dynamic Template是定义在某个索引的Mapping中
- Template有一个名称
- 匹配规则是一个数组
- 为匹配到字段设置Mapping

```sh
#结合路径
PUT my_index
{
  "mappings": {
    "dynamic_templates": [
      {
        "full_name": {
          "path_match":   "name.*",
          "path_unmatch": "*.middle",
          "mapping": {
            "type":       "text",
            "copy_to":    "full_name"
          }
        }
      }
    ]
  }
}
```