---
weight: 2
title: Dynamic Configuration
---

# Dynamic Configuration

## Get

```
GET /values
```

### Request

- Query
    - **keys** `string[]` Configuration key, empty return all

### Response

{{<hint info>}}

200 OK

- Body `object` Configurations

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

{{<tabs `get`>}}
{{<tab `Simple`>}}

```http
GET /values HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"Cloud":"tencent",...,"TencentSecretKey":"*"}
```

{{</tab>}}
{{<tab `Specify Key`>}}

```http
GET /values?keys=IpLoginFailures&keys=LoginFailures HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"IpLoginFailures":10,"LoginFailures":5}
```

{{</tab>}}
{{</tabs>}}

## Set

```
PATCH /values
```

### Request

- Body
    - **update** <font color="red">*required</font> `object` keyValue data

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

{{<tabs `patch`>}}
{{<tab `Simple`>}}

```http
PATCH /values HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "update": {
        "mytest": "hi"
    }
}

# Response

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```

{{</tab>}}
{{</tabs>}}

## Remove

```
DELETE /values/:key
```

### Request

- Path
    - **key** <font color="red">*required</font> `string` Configuration key

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

{{<tabs `delete`>}}
{{<tab `Simple`>}}

```http
DELETE /values/mytest HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```

{{</tab>}}
{{</tabs>}}
