# 地图经纬度查询

ES中提供了一个数据类型`geo_point`,这个类型就是用来存储经纬度的。

创建一个带`geo_point`类型的索引，并添加测试数据

```json
# 创建一个索引，指定一个name、location
PUT /map
{
  "settings": {
    "number_of_shards": 5,
    "number_of_replicas": 1
  },
  "mappings": {
    "map": {
      "properties": {
        "name": {
          "type": "text"
        },
        "location": {
          "type": "geo_point"
        }
      }
    }
  }
}

# 添加测试数据
PUT /map/map/1
{
  "name": "天安门",
  "location": {
    "lon": 116.403981,
    "lat": 39.914492
  }
}
```

## ES的地图检索方式

- geo_distance：直线距离检索方式
- geo_bounding_box：以两个点确定一个矩形，获取在矩形内的全部数据
- geo_polygon：以多个点，确定一个多边形，获取多边形内的全部数据

```json
# geo_distance
POST /map/map/_search
{
  "query": {
    "geo_distance": {
      "location": { # 确定一个点
        "lon": 116.433733,
        "lat": 39.908404
      },
      "distance": 3000, # 确定半径。单位：米
      "distance_type": "arc" # 指定形状为原型。类型：圆
    }
  }
}
```

```json
# geo_bounding_box
POST /map/map/_search
{
  "query": {
    "geo_bounding_box": {
      "location": {
        "top_left": {
          "lon": 116.326943,
          "lat": 39.95499
        },
        "bottom_right": {
          "lon": 116.433446,
          "lat": 39.908737
        }
      }
    }
  }
}
```

```json
# geo_polygon
POST /map/map/_search
{
  "query": {
    "geo_polygon": {
      "location": {
        "points": [ # 指定多个点确定一个多边形
          {
            "lon": 116.298916,
            "lat": 39.99878
          },
          {
            "lon": 116.29561,
            "lat": 39.972576
          },
          {
            "lon": 116.327661,
            "lat": 39.984739
          }
        ]
      }
    }
  }
}
```