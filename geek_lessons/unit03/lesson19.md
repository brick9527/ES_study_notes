# 第十九课 显示Mapping设置与常见参数介绍]

# 如何显示定义一个Mapping

```sh
PUT movies
{
  "mappings": {
    // 定义mapping
  }
}
```

# 自定义Mapping的一些建议

- 可以参考API手册，纯手写
- 为了减少输入的工作量，减少出错概率，可以依照以下步骤
  - 创建一个临时的Index，写入一些样本数据
  - 通过访问Mapping API获得该临时文件的动态Mapping定义
  - 修改后用，使用该配置创建你的索引
  - 删除临时索引

# 控制当前字段是否被索引

- Index - 控制当前字段是否被索引。默认为true。如果设置成false，该字段不可被搜索

```sh
#设置 index 为 false
PUT users
{
    "mappings" : {
      "properties" : {
        "firstName" : {
          "type" : "text"
        },
        "lastName" : {
          "type" : "text"
        },
        "mobile" : {
          "type" : "text",
          "index": false
        }
      }
    }
}
```

# Index Options

- 四种不同级别的Index Options配置，可以控制倒排索引记录的内容
  - docs：记录doc id
  - freqs：记录doc id和term frequencies
  - positions：记录doc id / term frequencies / term position
  - offsets：doc id /term frequencies / term position / character offects
- Text类型默认记录positions，其他默认为docs
- 记录内容越多，占用存储空间越大

# null_value

- 需要对Null值实现搜索
- 只有keyword类型支持设定Null_Value

```sh
#设定Null_value
PUT users
{
    "mappings" : {
      "properties" : {
        "firstName" : {
          "type" : "text"
        },
        "lastName" : {
          "type" : "text"
        },
        "mobile" : {
          "type" : "keyword",
          "null_value": "NULL"
        }

      }
    }
}
```

# copy_to设置

- _all在7中被copy_to所替代
- 满足一些特定的搜索需求
- copy_to将字段的数值拷贝到目标字段，实现类似_all的作用
- copy_to的目标字段不出现在_source中

```sh
#设置 Copy to
PUT users
{
  "mappings": {
    "properties": {
      "firstName":{
        "type": "text",
        "copy_to": "fullName"
      },
      "lastName":{
        "type": "text",
        "copy_to": "fullName"
      }
    }
  }
}
PUT users/_doc/1
{
  "firstName":"Ruan",
  "lastName": "Yiming"
}

# 可以直接使用copy_to之后的字段进行搜索
GET users/_search?q=fullName:(Ruan Yiming)
```

# 数组类型

- ES中不提供专门的数组类型。但是任何字段，都可以包含多个想同类型的数据
- 数组的mapping中type值依然是text

