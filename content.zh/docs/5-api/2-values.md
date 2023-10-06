---
weight: 2
title: 动态配置
---

# 动态配置

## 获取配置

```
GET /values
```

### Request

- Query
    - **keys** `string[]` 配置键，为空返回全部

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body `object` 配置数据

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

{{<tabs `get`>}}
{{<tab `简单示例`>}}

```http
GET /values HTTP/1.1
Host: api-x.kainonly.com:8443

返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"Cloud":"tencent",...,"TencentSecretKey":"*"}
```

{{</tab>}}
{{<tab `指定配置键`>}}

```http
GET /values?keys=IpLoginFailures&keys=LoginFailures HTTP/1.1
Host: api-x.kainonly.com:8443

返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"IpLoginFailures":10,"LoginFailures":5}
```

{{</tab>}}
{{</tabs>}}

## 更新配置

```
PATCH /values
```

### Request

- Body
    - **update** <font color="red">*必须</font> `object` 配置更新

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

{{<tabs `patch`>}}
{{<tab `简单示例`>}}

```http
PATCH /values HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "update": {
        "mytest": "hi"
    }
}

返回

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```

{{</tab>}}
{{</tabs>}}

## 删除配置

```
DELETE /values/:key
```

### Request

- Path
    - **key** <font color="red">*必须</font> `string` 配置键

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

{{<tabs `delete`>}}
{{<tab `简单示例`>}}

```http
DELETE /values/mytest HTTP/1.1
Host: api-x.kainonly.com:8443

返回

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```

{{</tab>}}
{{</tabs>}}
