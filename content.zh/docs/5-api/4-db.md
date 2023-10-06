---
weight: 4
title: 数据集
---

# 数据集

## 新增资源

```
POST /db/:collection/create
```

### Request

- Path
    - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
- Body
    - **data** <font color="red">*必须</font> `object` 资源数据
    - **xdata** `object` *Body.data* 格式转换
    - **txn** `string` 事务 ID

### Response

{{<hint info>}}

返回成功 - 201 Created

- Body
    - **InsertedID** `string` 新增资源 ID

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

{{<tabs `create`>}}
{{<tab `简单示例`>}}

```http
POST /db/x_departments/create HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "data": {
        "name": "客服组",
        "description": "客服总部门"
    }
}

# 返回

HTTP/1.1 201 Created
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Content-Type: application/json; charset=utf-8
Server: hertz

{
    "InsertedID": "651fe5b2199dfb8ca40ec524"
}
```

{{</tab>}}
{{<tab `数组示例`>}}

```http
POST /db/x_coupons/create HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "data": {
        "name": "体验卡",
        "pd": "2023-04-12T22:00:00.906Z",
        "valid": [
            "2023-04-12T22:00:00.906Z",
            "2023-04-13T06:30:05.586Z"
        ]
    },
    "xdata": {
        "pd": "timestamp",
        "valid": "timestamps"
    }
}

# 返回

HTTP/1.1 201 Created
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Content-Type: application/json; charset=utf-8
Server: hertz

{
    "InsertedID": "651fea5f199dfb8ca40ec525"
}
```

{{</tab>}}
{{<tab `格式转换示例`>}}

```http
POST /db/x_users/create HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "data": {
        "username": "weplanx",
        "password": "pass@VAN1234"
    },
    "format": {
        "password": "password"
    }
}

# 返回

HTTP/1.1 201 Created
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Content-Type: application/json; charset=utf-8
Server: hertz

{
    "InsertedID": "62db93de33c11192c28c61a3"
}
```

{{</tab>}}
{{</tabs>}}

## 批量新增资源

```
POST /db/:collection/bulk_create
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
- Body
  - **data** <font color="red">*必须</font> `object[]` 资源数据
  - **xdata** `object` *Body.data.$* 格式转换
  - **txn** `string` 事务 ID

### Response

{{<hint info>}}

返回成功 - 201 Created

- Body
  - **InsertedIDs** `string[]` 新增资源 ID

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `bulk_create`>}}
{{<tab `简单示例`>}}

```http
POST /db/x_coupons/bulk_create HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "data": [
        {
            "name": "体验卡A",
            "pd": "2023-04-12T22:00:00.906Z",
            "valid": [
                "2023-04-12T22:00:00.906Z",
                "2023-04-13T06:30:05.586Z"
            ]
        },
        {
            "name": "体验卡B",
            "pd": "2023-04-13T22:00:00.906Z",
            "valid": [
                "2023-04-13T22:00:00.906Z",
                "2023-04-14T06:30:05.586Z"
            ]
        },
        {
            "name": "体验卡C",
            "pd": "2023-04-15T22:00:00.906Z",
            "valid": [
                "2023-04-15T22:00:00.906Z",
                "2023-04-16T06:30:05.586Z"
            ]
        }
    ],
    "xdata": {
        "pd": "timestamp",
        "valid": "timestamps"
    }
}

# 返回

HTTP/1.1 201 Created
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Content-Type: application/json; charset=utf-8
Server: hertz

{
    "InsertedIDs": [
        "651fed56199dfb8ca40ec526",
        "651fed56199dfb8ca40ec527",
        "651fed56199dfb8ca40ec528"
    ]
}
```

{{</tab>}}
{{</tabs>}}

## 获取资源总数

```
POST /db/:collection/size
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
- Body
  - **filter** <font color="red">*必须</font> `object` 筛选条件
  - **xfilter** `object` *Body.filter* 格式转换

### Response

{{<hint info>}}

返回成功 - 204 No Content

- Header
  - **x-total** `string(number)`  资源总数

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `size`>}}
{{<tab `简单示例`>}}

```http
POST /db/x_orders/size HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

# 返回

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 5000
```

{{</tab>}}
{{<tab `筛选示例`>}}

```http
POST /db/x_orders/size HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "no": {
            "$in": [
                "4776797196163290",
                "4916502451560966"
            ]
        }
    }
}

# 返回

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 2
```

{{</tab>}}
{{<tab `格式转换示例`>}}

```http
POST /db/x_orders/size HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "_id": {
            "$in": [
                "64bdd1c042a8e3504975f04e"
            ]
        }
    },
    "xfilter": {
        "_id->$in": "oids"
    }
}

# 返回

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 1
```

{{</tab>}}
{{</tabs>}}

## 获取匹配资源

```
POST /db/:collection/find
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
- Header
  - **x-pagesize** `string(number)` 分页大小，默认 `100` 自定义必须在 `1~1000` 之间
  - **x-page** `string(number)` 分页页码
- Query
  - **sort** `string[]` 排序规则，格式为 `<field>:<1|-1>`
  - **keys** `string[]` 投影规则
- Body
  - **filter** <font color="red">*必须</font> `object` 筛选条件
  - **xfilter** `object` *Body.filter* 格式转换

### Response

{{<hint info>}}

返回成功 - 200 OK

- Header
  - **X-Total** `string(number)` 资源总数
- Body `object[]` 资源数据

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `find`>}}
{{<tab `简单示例`>}}

```http
POST /db/x_orders/find HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {}
}

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 5000

[ ... { "_id": "...", ... } 100 raw ... ]
```

{{</tab>}}
{{<tab `分页示例`>}}

```http
POST /db/x_orders/find HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json
x-page: 2
x-pagesize: 5

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 5000

[ ... { "_id": "...", ... } 5 raw ... ]
```

{{</tab>}}
{{<tab `筛选示例`>}}

```http
POST /db/x_orders/find HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "no": {
            "$in": [
                "4556795377247677",
                "4929152651590514"
            ]
        }
    }
}
```

{{</tab>}}
{{<tab `格式转换示例`>}}

```http
POST /db/x_departments/find HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "parent": "62db8e2133c11192c28c61a1"
    },
    "xfilter": {
        "parent": "oid"
    }
}
```

{{</tab>}}
{{<tab `投影示例`>}}

```http
POST /db/x_orders/find?keys=name HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {}
}
```

{{</tab>}}
{{<tab `排序示例`>}}

```http
POST /db/x_orders/find?sort=no:1 HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {}
}
```

{{</tab>}}
{{</tabs>}}

## 获取单个资源

```
POST /db/:collection/find_one
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
- Query
  - **keys** `string[]` 投影规则
- Body
  - **filter** <font color="red">*必须</font> `object` 筛选条件
  - **xfilter** `object` *Body.filter* 格式转换

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body `object` 资源数据

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `find-one`>}}
{{<tab `简单示例`>}}

```http
POST /db/x_orders/find_one HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "no": "3528731257638202"
    }
}

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"_id":"64bdd1c042a8e3504975f04e","account":"MfaoSxk","address":"Et velit veniam sed eos et.","create_time":"1994-05-02T13:34:34Z","customer":"King Jules Koelpin","description":"Quidem est est officiis velit est. Provident qui vero placeat eaque mollitia.","email":"jRUnefN@YLXvGfp.info","name":"Prof. Finn Wintheiser","no":"3528731257638202","phone":"104-215-9683","price":5956907.02,"update_time":"1994-05-03T13:34:34Z","valid":["2170-06-15T22:39:42.803Z","2265-03-22T21:48:37.854Z"]}
```

{{</tab>}}
{{<tab `格式转换示例`>}}

```http
POST /db/x_departments/find_one HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "parent": "62db8e2133c11192c28c61a1"
    },
    "xfilter": {
        "parent": "oid"
    }
}

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"_id":"62db95ec33c11192c28c61a7","name":"客服 B 组","parent":"62db8e2133c11192c28c61a1","create_time":"2022-07-23T14:32:12.502+08:00","update_time":"2022-07-23T14:32:12.502+08:00"}
```

{{</tab>}}
{{<tab `投影示例`>}}

```http
POST /db/x_orders/find_one?keys=no HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "no": "4566074530041583"
    }
}

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"_id":"62a455a4d2952c7033643764","no":"AZ14FFGW32000766490389800984"}
```

{{</tab>}}
{{</tabs>}}

## 获取指定 ID 的资源

```
GET /db/:collection/:id
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
  - **id** <font color="red">*必须</font> `string` 资源 ID，必须为 MongoId
- Query
  - **keys** `string[]` 投影规则

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body `object` 资源数据

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `find-by-id`>}}
{{<tab `简单示例`>}}

```http
GET /db/x_orders/64b14c68b26a163ca034ace7 HTTP/1.1
Host: api-x.kainonly.com:8443

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"create_time":"2022-03-08T17:12:28.607+08:00","_id":"64b14c68b26a163ca034ace7","no":"CY12008750579FE7390A801K60S7","name":"Fantastic Plastic Towels","price":980.94,"valid":["2021-09-04T00:23:26.91+08:00","2022-06-12T09:28:07.644+08:00"],"address":"3358 Lang Common","update_time":"2022-03-08T17:12:28.607+08:00","description":"Ergonomic executive chair upholstered in bonded black leather and PVC padded seat and back for all-day comfort and support","account":"94213614","customer":"Faye Hermann","email":"Sven20@hotmail.com","phone":"172828438"}
```

{{</tab>}}
{{<tab `投影示例`>}}

```http
GET /db/x_orders/64b14c68b26a163ca034ace7?keys=no HTTP/1.1
Host: api-x.kainonly.com:8443

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"_id":"64b14c68b26a163ca034ace7","no":"CY12008750579FE7390A801K60S7"}
```
{{</tab>}}
{{</tabs>}}

## 更新匹配资源

```
POST /db/:collection/update
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
- Body
  - **filter** <font color="red">*必须</font> `object` 筛选条件
  - **xfilter** `object` *Body.filter* 格式转换
  - **data** <font color="red">*必须</font> `object` 资源数据
  - **xdata** `object` *Body.data* 格式转换
  - **txn** `string` 事务 ID

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body
  - **MatchedCount** `number` 匹配数量
  - **ModifiedCount** `number` 修改数量
  - **UpsertedCount** `number` 插入更新数量
  - **UpsertedID** `string` 插入更新的 ID

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `update`>}}
{{<tab `简单示例`>}}

```http
POST /db/x_departments/update HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "_id": {
            "$in": [
                "64b14d216fd369656e4ad62f",
                "64b14eb16fd369656e4ad632"
            ]
        }
    },
    "xfilter": {
        "_id->$in": "oids"
    },
    "data": {
        "$set": {
            "status": true
        }
    }
}

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"MatchedCount":2,"ModifiedCount":2,"UpsertedCount":0,"UpsertedID":null}
```

{{</tab>}}
{{<tab `格式转换示例`>}}

```http
POST /db/x_departments/update HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "_id": {
            "$in": [
                "64b14d216fd369656e4ad62f",
                "64b14eb16fd369656e4ad632"
            ]
        }
    },
    "xfilter": {
        "_id->$in": "oids"
    },
    "data": {
        "$set": {
            "parent": "62db95ec33c11192c28c61a8"
        }
    },
    "xdata": {
        "$set->parent": "oid"
    }
}

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"MatchedCount":2,"ModifiedCount":2,"UpsertedCount":0,"UpsertedID":null}
```

{{</tab>}}
{{</tabs>}}

## 更新指定 ID 的资源

```
PATCH /db/:collection/:id
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
  - **id** <font color="red">*必须</font> `string` 资源 ID，必须为 MongoId
- Body
  - **data** <font color="red">*必须</font> `object` 资源数据
  - **xdata** `object` *Body.data* 格式转换
  - **txn** `string` 事务 ID

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body
  - **MatchedCount** `number` 匹配数量
  - **ModifiedCount** `number` 修改数量
  - **UpsertedCount** `number` 插入更新数量
  - **UpsertedID** `string` 插入更新的 ID

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `update-by-id`>}}
{{<tab `简单示例`>}}

```http
PATCH /db/x_users/64b14e776fd369656e4ad631 HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "data": {
        "$set": {
            "roles": [
                "62890dfd9491cd1f5a0a7082",
                "62a9dfb2e4354d6b89337122"
            ]
        }
    },
    "xdata": {
        "$set->roles": "oids"
    }
}

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"MatchedCount":1,"ModifiedCount":1,"UpsertedCount":0,"UpsertedID":null}
```

{{</tab>}}
{{</tabs>}}

## 替换指定 ID 的资源

```
PUT /db/:collection/:id
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
  - **id** <font color="red">*必须</font> `string` 资源 ID，必须为 MongoId
- Body
  - **data** <font color="red">*必须</font> `object` 资源数据
  - **xdata** `object` *Body.data* 格式转换
  - **txn** `string` 事务 ID

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body
  - **MatchedCount** `number` 匹配数量
  - **ModifiedCount** `number` 修改数量
  - **UpsertedCount** `number` 插入更新数量
  - **UpsertedID** `string` 插入更新的 ID

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `replace`>}}
{{<tab `简单示例`>}}

```http
PUT /db/x_users/64b14e776fd369656e4ad631 HTTP/1.1
Host: xapi.kainonly.com:8443
Content-Type: application/json

{
    "data": {
        "username": "weplanx",
        "password": "pass@VAN1234",
        "department": "62db95ec33c11192c28c61a8",
        "roles": [
            "62890dfd9491cd1f5a0a7082"
        ],
        "metadata": {
            "x": {
                "time": "Sat, 23 Jul 2022 08:44:09 GMT"
            }
        }
    },
    "xdata": {
        "password": "password",
        "department": "oid",
        "roles": "oids",
        "metadata->x->time": "date"
    }
}

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"MatchedCount":1,"ModifiedCount":1,"UpsertedCount":0,"UpsertedID":null}
```

{{</tab>}}
{{</tabs>}}

## 删除指定 ID 的资源

```
DELETE /db/:collection/:id
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
  - **id** <font color="red">*必须</font> `string` 资源 ID，必须为 MongoId
- Query
  - **txn** `string` 事务 ID

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body
  - **DeletedCount** 删除数量

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `delete`>}}
{{<tab `简单示例`>}}

```http
DELETE /db/x_users/64b14e776fd369656e4ad631 HTTP/1.1
Host: api-x.kainonly.com:8443

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":1}
```

{{</tab>}}
{{</tabs>}}

## 批量删除匹配资源

```
POST /db/:collection/bulk_delete
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
- Body
  - **filter** <font color="red">*必须</font> `object` 筛选条件
  - **xfilter** `object` *Body.filter* 格式转换
  - **txn** `string` 事务 ID

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body
  - **DeletedCount** 删除数量

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `bulk-delete`>}}
{{<tab `简单示例`>}}

```http
POST /db/x_departments/bulk_delete HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "_id": {
            "$in": [
                "64b14d216fd369656e4ad62f",
                "64b14eb16fd369656e4ad632"
            ]
        }
    },
    "xfilter": {
        "_id->$in": "oids"
    }
}

# 返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":2}
```

{{</tab>}}
{{</tabs>}}

## 排序资源

```
POST /db/:collection/sort
```

### Request

- Path
  - **collection** <font color="red">*必须</font> `string` 集合名称，必须是小写字母与下划线
- Body
  - **data** <font color="red">*必须</font> `object` 排序数据
    - **key** `string` 排序字段，例如：`sort`
    - **values** `string[]` 排序的 ID 数组，ID 必须为 MongoId，数组索引即顺序
  - **txn** `string` 事务 ID

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `sort`>}}
{{<tab `简单示例`>}}

```http
POST /db/x_departments/sort HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "data": {
        "key": "sort",
        "values": [
            "64b14eb16fd369656e4ad634",
            "64b14eb16fd369656e4ad633"
        ]
    }
}
```

{{</tab>}}
{{</tabs>}}

## 发起事务

```
POST /db/transaction
```

### Response

{{<hint info>}}

返回成功 - 201 Created

- Body
  - **txn** `string` Txn 事务 ID

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

## 提交事务

```
POST /db/commit
```

### Request

- Body
  - **txn** `string` 事务 ID

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body `any[]`

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `transaction`>}}
{{<tab `1-发起事务`>}}

```http
POST /db/transaction HTTP/1.1
Host: api-x.kainonly.com:8443

返回

HTTP/1.1 201 Created
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"txn": "499feb97-e4ae-4bc8-96e6-78f55e589687"}
```

{{</tab>}}
{{<tab `2-事务新增权限组`>}}

```http
POST /db/x_roles/create HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "data": {
        "name": "超级管理员",
        "key": "*"
    },
    "txn": "499feb97-e4ae-4bc8-96e6-78f55e589687"
}

返回

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```

{{</tab>}}
{{<tab `3-事务新增用户`>}}

```http
POST /db/x_users/create HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "data": {
        "name": "kain",
        "roles": [
            "*"
        ]
    },
    "txn": "499feb97-e4ae-4bc8-96e6-78f55e589687"
}

返回

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```

{{</tab>}}
{{<tab `4-提交事务`>}}

```http
POST /db/commit HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

[{"InsertedID":"65200958199dfb8ca40ec52a"},{"InsertedID":"65200958199dfb8ca40ec52b"}]
```

{{</tab>}}
{{</tabs>}}
