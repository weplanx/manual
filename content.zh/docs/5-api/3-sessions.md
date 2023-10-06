---
weight: 3
title: 在线会话
---

# 在线会话

## 获取会话

```
GET /sessions
```

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body `[]string` 用户 ID 数组

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

{{<tabs `get`>}}
{{<tab `简单示例`>}}

```http
GET /sessions HTTP/1.1
Host: api-x.kainonly.com:8443

返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

["64bfbe69eddf04482dcf9d43","64ae547e5a94adff829274c7"]
```

{{</tab>}}
{{</tabs>}}

## 中断指定会话

```
DELETE /sessions/:uid
```

### Request

- Path
    - **uid** <font color="red">*必须</font> `string` 用户 ID，必须为 MongoId

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
DELETE /sessions/64bfbe69eddf04482dcf9d43 HTTP/1.1
Host: api-x.kainonly.com:8443

返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":1}
```

{{</tab>}}
{{</tabs>}}

## 中断所有会话

```
POST /sessions/clear
```

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

{{<tabs `clear`>}}
{{<tab `简单示例`>}}

```http
POST /sessions/clear HTTP/1.1
Host: api-x.kainonly.com:8443

返回

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":2}
```

{{</tab>}}
{{</tabs>}}

