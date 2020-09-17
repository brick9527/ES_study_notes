# 第九课 基本概念：索引、文档和REST API

# 一、文档（Document）

- es是面向文档的，文档是所有可搜索数据的最小单位
  - 日志文件中的日志项
  - 一本电影的具体信息 / 一张唱片的详细信息
  - MP3播放器里的一首歌 / 一篇PDF文档中的具体内容
- 文档会被序列化成JSON格式，保存在es中
  - JSON对象由字段组成
  - 每个字段都有对应的字段类型（字符串 / 数值 / 布尔 / 日期 / 二进制 / 范围类型）
- 每个文档都有一个Unique ID
  - 你可以自己指定ID
  - 或者通过es自动生成
  
# 二、文档的元数据

- 元数据：用于标注文档的相关信息
  - _index: 文档所属的索引名
  - _type: 文档所属的类型名
  - _id: 文档唯一ID
  - _source: 文档的原始JSON数据
  - _all: 整合所有字段内容到该字段（已被废除， 7.0开始 废除）
  - _version: 文档的版本信息
  - _score: 相关性打分

# 三、索引

- index：索引是文档的容器，是一类文档的结合
  - index体现了逻辑空间的概念：每个索引都有自己的Mapping定义，用于定义包含的字段名和字段类型
  - Shard体现了物理空间的概念：索引中的数据分散在Shard上
- 索引的Mapping与Settings
  - Mapping定义文档字段的类型
  - Setting定义不同的数据分布

# 四、索引的不同语义

- 名词：一个ES集群中，可以创建很多个不同的索引
- 动词：保存一个文档到ES的过程也叫索引（indexing）
- 名词：一个B数索引，一个倒排索引

# 五、Type

- 7.0之前，一个Index可以有设置多个Type
- 6.0开始，Type已经被Deprecated。7.0开始，一个索引只能创建一个Type-"_doc"

# 六、抽象与类比

|RDBMS|ES|
|-|-|
|Table|Index（Type）|
|Row|Document|
|Column|Field|
|Schema|Mapping|
|SQL|DSL|