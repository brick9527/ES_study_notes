# 索引的操作

## 创建一个索引

```json
PUT /person
{
  "settings": {
    "number_of_shards": 5, // 分片数为5
    "number_of_replicas": 1 // 备份数为1
  }
}
```

## 查看索引

```json
GET /person
```

## 删除索引

```json
DELETE /person
```