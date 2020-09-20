# 第十八课 Dynamic Mapping和常见字段类型

# 什么是Mapping

- Mapping类似数据库中的schema的定义，作用如下：
  - 定义索引中的字段的名称
  - 定义字段的数据类型，例如字符串，数字，布尔……
  - 字段，倒排索引的相关配置，（Analyzed or Not Analyzed, Analyzer）
- Mapping会把JSON文档映射成Lucene所需要的扁平格式
- 一个Mapping属于一个索引的Type
  - 每个文档都属于一个Type
  - 一个Type有一个Mapping定义
  - 7.0开始，不需要在Mapping定义中指定type信息

# 字段的数据类型

- 简单类型
  - Text / Keyword
  - Date
  - Integer / Floating
  - Boolean
  - IPv4 & IPv6
- 复杂类型 - 对象和嵌套类型
  - 对象类型 / 嵌套类型
- 特殊类型
  - gep_point & geo_shape & percolator

# 什么是Dynamic Mapping

- 在写入文档时候，如果索引不存在，会自动创建索引
- Dynamic Mapping的机制，使得我们无需手动定义Mappings。ES会自动根据文档信息，推算出字段的类型
- 但是有时候会推算的不对，例如地理位置信息
- 当类型如果设置不对时，会导致一些功能无法正常运行，例如range查询

查看Mapping

```sh
GET movies/_mappings
```

# 类型的自动识别

|JSON类型|ES类型|
|-|-|
|字符串|1. 匹配日期格式，设置成Date <br>2. 配置数字设置为float或者long，该选项默认关闭 <br>3. 设置为Text，并且增加keyword子字段|
|布尔值|boolean|
|浮点数|float|
|整数|long|
|对象|Object|
|数组|由第一个非空数值的类型所决定|
|空值|忽略|

# 能否更改Mapping的字段类型

- 两种情况
  - 新增加字段
    - Dynamic设为true时，一旦有新增字段的文档写入，Mapping也同时被更新
    - Dynamic设为false，Mapping不会被更新，新增字段的数据无法被索引（无法被搜索），但是信息会出现在_source中
    - Dynamic设置成Strict，文档写入失败
  - 对已有字段，一旦已经有数据写入，就不再支持修改字段定义
    - Lucene实现的倒排索引，一旦生成后，就不允许修改
  - 如果希望改变字段类型，必须Reindex API，重建索引
- 原因
  - 如果修改了字段的数据类型，会导致已被索引的数据无法被搜索
  - 但是如果是增加新的字段，就不会有这样的影响

# 控制Dynamic Mappings

||“true”|“false”|“strict”|
|-|-|-|-|
|文档可索引|YES|YES|NO|
|字段可索引|YES|NO|NO|
|Mapping被更新|YES|NO|NO|

- 当dynamic被设置成false的时候，存在新增字段的数据写入，该数据可以被索引，但是新增字段被丢弃
- 当设置成Strict模式时，数据写入直接报错

# demo

1. 先写入一个数据
2. 修改文档的dynamic为false
3. 添加一个数据，新字段
4. 查询这个新字段，没有结果；查看文档的mapping信息，没有这个字段的索引
5. dynamic改为strict
6. 添加一个数据，新字段，报错