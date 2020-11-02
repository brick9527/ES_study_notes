# ES的RESTFul语法

## GET请求

- http://ip:port/index: 查询索引信息
- http://ip:port/index/type/doc_id: 查询指定的文档信息

## POST请求

- http://ip:port/index/type/_search: 查询文档，可以在请求体中添加json字符串来代表查询条件
- http://ip:port/index/type/doc_id/_update: 修改文档，在请求体中指定json字符串代表修改的具体信息

## PUT请求

- http://ip:port/index: 创建一个索引，需要在请求体中指定索引的信息，类型，结构
- http://ip:port/index/type/_mappings: 代表创建索引时，指定索引文档存储的属性的信息(mapping)

## DELETE请求

- http://ip:port/index: 删除一个index
- http://ip:port/index/type/doc_id: 删除一个doc


