---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Mongo Rest

{% swagger method="post" path="" baseUrl="/db/:collection/create" summary="Create" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="body" type="Object" name="data" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xdata" type="Object" %}
_Body.data_ format conversion
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txn" type="String" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="" %}
```
- Body
  - **InsertedID** `string`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}

{% tab title="Array" %}
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
{% endtab %}

{% tab title="Format" %}
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
{% endtab %}
{% endtabs %}

{% swagger method="post" path="" baseUrl="/db/:collection/bulk_create" summary="BulkCreate" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" required="true" type="String" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" type="Object[]" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xdata" type="Object" %}
_Body.data.$_ format conversion
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txn" type="String" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="201: Created" description="" %}
```
- Body
  - **InsertedIDs** `string[]`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}
{% endtabs %}

{% swagger method="post" path="" baseUrl="/db/:collection/size" summary="Size" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="filter" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xfilter" type="Object" %}
_Body.filter_ format conversion
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="" %}
```
- Header
  - **x-total** `string(number)` Total
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}

{% tab title="Filter" %}
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
{% endtab %}

{% tab title="Format" %}
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
{% endtab %}
{% endtabs %}

{% swagger method="post" path="" baseUrl="/db/:collection/find" summary="Find" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="header" name="x-pagesize" type="Number" %}
Paging size, default `100` customize must be between `1~1000`
{% endswagger-parameter %}

{% swagger-parameter in="header" name="x-page" type="Number" %}
Paging Index
{% endswagger-parameter %}

{% swagger-parameter in="query" name="sort" type="String[]" %}
Sort rules `<field>:<1|-1>`
{% endswagger-parameter %}

{% swagger-parameter in="query" name="keys" type="String[]" %}
Projection
{% endswagger-parameter %}

{% swagger-parameter in="body" name="filter" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xfilter" type="Object" %}
_Body.filter_ format conversion
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Header
  - **X-Total** `string(number)` Total
- Body `object[]` Data array
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}

{% tab title="Paging" %}
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
{% endtab %}

{% tab title="Filter" %}
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
{% endtab %}

{% tab title="Format" %}
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
{% endtab %}

{% tab title="Projection" %}
```http
POST /db/x_orders/find?keys=name HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {}
}
```
{% endtab %}

{% tab title="Sort" %}
```http
POST /db/x_orders/find?sort=no:1 HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "filter": {}
}
```
{% endtab %}
{% endtabs %}

{% swagger method="post" path="" baseUrl="/db/:collection/find_one" summary="FindOne" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="query" name="keys" type="String[]" %}
Projection
{% endswagger-parameter %}

{% swagger-parameter in="body" name="filter" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xfilter" type="Object" %}
_Body.filter_ format conversion
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body `object`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}

{% tab title="Format" %}
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
{% endtab %}

{% tab title="Projection" %}
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
{% endtab %}
{% endtabs %}

{% swagger method="get" path="" baseUrl="/db/:collection/:id" summary="FindById" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
ID, must be MongoId
{% endswagger-parameter %}

{% swagger-parameter in="query" name="keys" type="String[]" %}
Projection
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body `object`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
```http
GET /db/x_orders/64b14c68b26a163ca034ace7 HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"create_time":"2022-03-08T17:12:28.607+08:00","_id":"64b14c68b26a163ca034ace7","no":"CY12008750579FE7390A801K60S7","name":"Fantastic Plastic Towels","price":980.94,"valid":["2021-09-04T00:23:26.91+08:00","2022-06-12T09:28:07.644+08:00"],"address":"3358 Lang Common","update_time":"2022-03-08T17:12:28.607+08:00","description":"Ergonomic executive chair upholstered in bonded black leather and PVC padded seat and back for all-day comfort and support","account":"94213614","customer":"Faye Hermann","email":"Sven20@hotmail.com","phone":"172828438"}
```
{% endtab %}

{% tab title="Projection" %}
```http
GET /db/x_orders/64b14c68b26a163ca034ace7?keys=no HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"_id":"64b14c68b26a163ca034ace7","no":"CY12008750579FE7390A801K60S7"}
```
{% endtab %}
{% endtabs %}

{% swagger method="post" path="" baseUrl="/db/:collection/update" summary="Update" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="filter" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xfilter" type="Object" %}
_Body.filter_ format conversion
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xdata" type="Object" %}
_Body.data_ format conversion
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txn" type="String" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body
  - **MatchedCount** `number`
  - **ModifiedCount** `number`
  - **UpsertedCount** `number`
  - **UpsertedID** `string`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}

{% tab title="Format" %}
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
{% endtab %}
{% endtabs %}

{% swagger method="patch" path="" baseUrl="/db/:collection/:id" summary="UpdateById" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
ID, must be MongoId
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xdata" type="Object" %}
_Body.data_ format conversion
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txn" type="String" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body
  - **MatchedCount** `number`
  - **ModifiedCount** `number`
  - **UpsertedCount** `number`
  - **UpsertedID** `string`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}
{% endtabs %}

{% swagger method="put" path="" baseUrl="/db/:collection/:id" summary="Replace" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
ID, must be MongoId
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xdata" type="Object" %}
_Body.data_ format conversion
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txn" type="String" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body
  - **MatchedCount** `number`
  - **ModifiedCount** `number`
  - **UpsertedCount** `number`
  - **UpsertedID** `string`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}
{% endtabs %}

{% swagger method="delete" path="" baseUrl="/db/:collection/:id" summary="Delete" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
ID, must be MongoId
{% endswagger-parameter %}

{% swagger-parameter in="query" name="txn" type="String" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body
  - **DeletedCount**
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
```http
DELETE /db/x_users/64b14e776fd369656e4ad631 HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":1}
```
{% endtab %}
{% endtabs %}

{% swagger method="post" path="" baseUrl="/db/:collection/bulk_delete" summary="BulkDelete" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="filter" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="xfilter" type="Object" %}
_Body.filter_ format conversion
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txn" type="String" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body
  - **DeletedCount**
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}
{% endtabs %}

{% swagger method="get" path="" baseUrl="/db/:collection/sort" summary="Sort" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="collection" type="String" required="true" %}
Collection name, lowercase letters and underscores allowed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="data.key" type="String" required="true" %}
Sorting field, for example: `sort`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data.values" type="String[]" required="true" %}
Sorted ID array, ID must be MongoId, array index is order
{% endswagger-parameter %}

{% swagger-parameter in="body" name="txn" type="String" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="" %}

{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
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
{% endtab %}
{% endtabs %}

{% swagger method="post" path="" baseUrl="/db/transaction" summary="Transaction" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="201: Created" description="" %}
```
- Body
  - **txn** `string` Txn Transaction ID
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}



{% swagger method="post" path="" baseUrl="/db/commit" summary="Commit" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="txn" type="String" required="true" %}
Transaction ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body `any[]`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Initiate" %}
```http
POST /db/transaction HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 201 Created
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"txn": "499feb97-e4ae-4bc8-96e6-78f55e589687"}
```
{% endtab %}

{% tab title="Add role" %}
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
{% endtab %}

{% tab title="Add user" %}
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
{% endtab %}

{% tab title="Commit" %}
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
{% endtab %}
{% endtabs %}
