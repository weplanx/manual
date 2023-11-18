---
weight: 4
title: Mongo Rest
---

# Mongo Rest

## Create

```
POST /db/:collection/create
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
- Body
  - **data** <font color="red">\*required</font> `object`
  - **xdata** `object` _Body.data_ format conversion
  - **txn** `string` Transaction ID

### Response

{{<hint info>}}

201 Created

- Body
  - **InsertedID** `string`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `create`>}}
{{<tab `Simple`>}}

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

# Response

HTTP/1.1 201 Created
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Content-Type: application/json; charset=utf-8
Server: hertz

{
    "InsertedID": "651fe5b2199dfb8ca40ec524"
}
```

{{</tab>}}
{{<tab `Array`>}}

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

# Response

HTTP/1.1 201 Created
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Content-Type: application/json; charset=utf-8
Server: hertz

{
    "InsertedID": "651fea5f199dfb8ca40ec525"
}
```

{{</tab>}}
{{<tab `Format`>}}

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

# Response

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

## BulkCreate

```
POST /db/:collection/bulk_create
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
- Body
  - **data** <font color="red">\*required</font> `object[]`
  - **xdata** `object` _Body.data.$_ format conversion
  - **txn** `string` Transaction ID

### Response

{{<hint info>}}

201 Created

- Body
  - **InsertedIDs** `string[]`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `bulk_create`>}}
{{<tab `Simple`>}}

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

# Response

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

## Size

```
POST /db/:collection/size
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
- Body
  - **filter** <font color="red">\*required</font> `object`
  - **xfilter** `object` _Body.filter_ format conversion

### Response

{{<hint info>}}

204 No Content

- Header
  - **x-total** `string(number)` Total

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `size`>}}
{{<tab `Simple`>}}

```http
POST /db/x_orders/size HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

# Response

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 5000
```

{{</tab>}}
{{<tab `Filter`>}}

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

# Response

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 2
```

{{</tab>}}
{{<tab `Format`>}}

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

# Response

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 1
```

{{</tab>}}
{{</tabs>}}

## Find

```
POST /db/:collection/find
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
- Header
  - **x-pagesize** `string(number)` Paging size, default `100` customize must be between `1~1000`
  - **x-page** `string(number)` Paging Index
- Query
  - **sort** `string[]` Sort rules `<field>:<1|-1>`
  - **keys** `string[]` Projection
- Body
  - **filter** <font color="red">\*required</font> `object`
  - **xfilter** `object` _Body.filter_ format conversion

### Response

{{<hint info>}}

200 OK

- Header
  - **X-Total** `string(number)` Total
- Body `object[]` Data array

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `find`>}}
{{<tab `Simple`>}}

```http
POST /db/x_orders/find HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {}
}

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 5000

[ ... { "_id": "...", ... } 100 raw ... ]
```

{{</tab>}}
{{<tab `Paging`>}}

```http
POST /db/x_orders/find HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json
x-page: 2
x-pagesize: 5

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
X-Total: 5000

[ ... { "_id": "...", ... } 5 raw ... ]
```

{{</tab>}}
{{<tab `Filter`>}}

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
{{<tab `Format`>}}

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
{{<tab `Projection`>}}

```http
POST /db/x_orders/find?keys=name HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {}
}
```

{{</tab>}}
{{<tab `Sort`>}}

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

## FindOne

```
POST /db/:collection/find_one
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
- Query
  - **keys** `string[]` Projection
- Body
  - **filter** <font color="red">\*required</font> `object`
  - **xfilter** `object` _Body.filter_ format conversion

### Response

{{<hint info>}}

200 OK

- Body `object`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `find-one`>}}
{{<tab `Simple`>}}

```http
POST /db/x_orders/find_one HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "no": "3528731257638202"
    }
}

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"_id":"64bdd1c042a8e3504975f04e","account":"MfaoSxk","address":"Et velit veniam sed eos et.","create_time":"1994-05-02T13:34:34Z","customer":"King Jules Koelpin","description":"Quidem est est officiis velit est. Provident qui vero placeat eaque mollitia.","email":"jRUnefN@YLXvGfp.info","name":"Prof. Finn Wintheiser","no":"3528731257638202","phone":"104-215-9683","price":5956907.02,"update_time":"1994-05-03T13:34:34Z","valid":["2170-06-15T22:39:42.803Z","2265-03-22T21:48:37.854Z"]}
```

{{</tab>}}
{{<tab `Format`>}}

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

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"_id":"62db95ec33c11192c28c61a7","name":"客服 B 组","parent":"62db8e2133c11192c28c61a1","create_time":"2022-07-23T14:32:12.502+08:00","update_time":"2022-07-23T14:32:12.502+08:00"}
```

{{</tab>}}
{{<tab `Projection`>}}

```http
POST /db/x_orders/find_one?keys=no HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {
        "no": "4566074530041583"
    }
}

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"_id":"62a455a4d2952c7033643764","no":"AZ14FFGW32000766490389800984"}
```

{{</tab>}}
{{</tabs>}}

## FindById

```
GET /db/:collection/:id
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
  - **id** <font color="red">\*required</font> `string` ID, must be MongoId
- Query
  - **keys** `string[]` Projection

### Response

{{<hint info>}}

200 OK

- Body `object`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `find-by-id`>}}
{{<tab `Simple`>}}

```http
GET /db/x_orders/64b14c68b26a163ca034ace7 HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"create_time":"2022-03-08T17:12:28.607+08:00","_id":"64b14c68b26a163ca034ace7","no":"CY12008750579FE7390A801K60S7","name":"Fantastic Plastic Towels","price":980.94,"valid":["2021-09-04T00:23:26.91+08:00","2022-06-12T09:28:07.644+08:00"],"address":"3358 Lang Common","update_time":"2022-03-08T17:12:28.607+08:00","description":"Ergonomic executive chair upholstered in bonded black leather and PVC padded seat and back for all-day comfort and support","account":"94213614","customer":"Faye Hermann","email":"Sven20@hotmail.com","phone":"172828438"}
```

{{</tab>}}
{{<tab `Projection`>}}

```http
GET /db/x_orders/64b14c68b26a163ca034ace7?keys=no HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"_id":"64b14c68b26a163ca034ace7","no":"CY12008750579FE7390A801K60S7"}
```

{{</tab>}}
{{</tabs>}}

## Update

```
POST /db/:collection/update
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
- Body
  - **filter** <font color="red">\*required</font> `object`
  - **xfilter** `object` _Body.filter_ format conversion
  - **data** <font color="red">\*required</font> `object`
  - **xdata** `object` _Body.data_ format conversion
  - **txn** `string` Transaction ID

### Response

{{<hint info>}}

200 OK

- Body
  - **MatchedCount** `number`
  - **ModifiedCount** `number`
  - **UpsertedCount** `number`
  - **UpsertedID** `string`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `update`>}}
{{<tab `Simple`>}}

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

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"MatchedCount":2,"ModifiedCount":2,"UpsertedCount":0,"UpsertedID":null}
```

{{</tab>}}
{{<tab `Format`>}}

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

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"MatchedCount":2,"ModifiedCount":2,"UpsertedCount":0,"UpsertedID":null}
```

{{</tab>}}
{{</tabs>}}

## UpdateById

```
PATCH /db/:collection/:id
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
  - **id** <font color="red">\*required</font> `string` ID, must be MongoId
- Body
  - **data** <font color="red">\*required</font> `object`
  - **xdata** `object` _Body.data_ format conversion
  - **txn** `string` Transaction ID

### Response

{{<hint info>}}

200 OK

- Body
  - **MatchedCount** `number`
  - **ModifiedCount** `number`
  - **UpsertedCount** `number`
  - **UpsertedID** `string`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `update-by-id`>}}
{{<tab `Simple`>}}

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

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"MatchedCount":1,"ModifiedCount":1,"UpsertedCount":0,"UpsertedID":null}
```

{{</tab>}}
{{</tabs>}}

## Replace

```
PUT /db/:collection/:id
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
  - **id** <font color="red">\*required</font> `string` ID, must be MongoId
- Body
  - **data** <font color="red">\*required</font> `object`
  - **xdata** `object` _Body.data_ format conversion
  - **txn** `string` Transaction ID

### Response

{{<hint info>}}

200 OK

- Body
  - **MatchedCount** `number`
  - **ModifiedCount** `number`
  - **UpsertedCount** `number`
  - **UpsertedID** `string`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `replace`>}}
{{<tab `Simple`>}}

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

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"MatchedCount":1,"ModifiedCount":1,"UpsertedCount":0,"UpsertedID":null}
```

{{</tab>}}
{{</tabs>}}

## Delete

```
DELETE /db/:collection/:id
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
  - **id** <font color="red">\*required</font> `string` ID, must be MongoId
- Query
  - **txn** `string` Transaction ID

### Response

{{<hint info>}}

200 OK

- Body
  - **DeletedCount**

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `delete`>}}
{{<tab `Simple`>}}

```http
DELETE /db/x_users/64b14e776fd369656e4ad631 HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":1}
```

{{</tab>}}
{{</tabs>}}

## BulkDelete

```
POST /db/:collection/bulk_delete
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
- Body
  - **filter** <font color="red">\*required</font> `object`
  - **xfilter** `object` _Body.filter_ format conversion
  - **txn** `string` Transaction ID

### Response

{{<hint info>}}

200 OK

- Body
  - **DeletedCount**

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `bulk-delete`>}}
{{<tab `Simple`>}}

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

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":2}
```

{{</tab>}}
{{</tabs>}}

## Sort

```
POST /db/:collection/sort
```

### Request

- Path
  - **collection** <font color="red">\*required</font> `string` Collection name, lowercase letters and underscores allowed
- Body
  - **data** <font color="red">\*required</font> `object`
    - **key** `string` Sorting field, for example: `sort`
    - **values** `string[]` Sorted ID array, ID must be MongoId, array index is order
  - **txn** `string` Transaction ID

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `sort`>}}
{{<tab `Simple`>}}

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

## Transaction

```
POST /db/transaction
```

### Response

{{<hint info>}}

201 Created

- Body
  - **txn** `string` Txn Transaction ID

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

## Commit

```
POST /db/commit
```

### Request

- Body
  - **txn** `string` Transaction ID

### Response

{{<hint info>}}

200 OK

- Body `any[]`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `transaction`>}}
{{<tab `Initiate`>}}

```http
POST /db/transaction HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 201 Created
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"txn": "499feb97-e4ae-4bc8-96e6-78f55e589687"}
```

{{</tab>}}
{{<tab `Add role`>}}

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

# Response

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```

{{</tab>}}
{{<tab `Add user`>}}

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

# Response

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```

{{</tab>}}
{{<tab `Commit`>}}

```http
POST /db/commit HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

[{"InsertedID":"65200958199dfb8ca40ec52a"},{"InsertedID":"65200958199dfb8ca40ec52b"}]
```

{{</tab>}}
{{</tabs>}}
