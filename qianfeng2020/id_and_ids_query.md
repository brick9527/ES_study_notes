# id & ids查询

## id查询

根据id查询，`where id =?`

```json
GET /sms-logs-index/sms-logs-type/1
```

## ids查询

根据多个id查询，类似MySQL中的`where id in (id1, id2, id3...)`

```json
POST /sms-logs-index/sms-logs-type/_search
{
  "query": {
    "ids": {
      "values": ["1", "2", "3"]
    }
  }
}
```