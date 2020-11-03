# ES中Field的类型

## 字符串类型

- text：一般被用于全文检索，将当前Field进行分词
- keyword：当前Field不会被分词

## 数值类型

- long：
- integer
- short
- byte
- double
- float
- half_float：精度比float小一半
- scaled_float：根据一个long和scaled来表达一个浮点型。long：345，scaled：100->3.45

## 事件类型

- date类型，针对时间类型指定具体的格式

## 布尔类型

- boolean类型，表达true和false

## 二进制类型

- binary类型暂时支持Base64 encode string

## 范围类型

- long_range：赋值时，无需指定具体的内容，只需要存储一个范围即可，指定`gt`,`lt`,`gte`,`lte`
- integer_range：同上
- double_range：同上
- float_range：同上
- date_range：同上
- ip_range：同上

## 经纬度类型

- geo_point：用来存储经纬度的

## ip类型

- ip：可以存储IPv4或者IPv6

其他数据类型参考官方文档