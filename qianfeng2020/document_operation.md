# ES关于文档的操作

文档在ES服务中的唯一标识，`_index`,`_type`,`_id`三个内容为组合，锁定一个文档，操作时添加还是修改。

## 新建文档

- 自动生成_id

```json
# 添加文档，自动生成_id
POST /book/novel
{
  "name": "张三",
  "age": 20
}
```

- 手动指定_id
  
```json
# 添加文档，手动指定_id
PUT /book/novel/1
{
  "name": "李四",
  "age": 88
}
```

## 修改文档

- 覆盖式修改

```json
# 修改文档，覆盖之前的
PUT /book/novel/1
{
  "name": "王五"
}
```

- doc修改方式

```json
POST /book/novel/1/_update
{
  "doc": {
    # 指定需要修改的字段和值
    "age": 18
  }
}
```

## 删除文档

```json
# 根据id删除文档
DELETE /book/novel/1
```