---
weight: 1
title: EMQX HTTP
---

# EMQX HTTP

## 认证

```
POST /emqx/auth
```

### Request

- Body
  - **identity** `string` 即时通讯授权项目的 MongoId
  - **token** `string` 所属项目签发的 Token

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 授权

```
POST /emqx/acl
```

### Request

- Body
    - **identity** `string` 即时通讯授权项目的 MongoId
    - **topic** `string` 即时通信主题

### Response

{{<hint info>}}

返回成功（授权通过） - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回成功（授权拒绝） - 200 OK

- Body
    - **result** `string` = "deny" 时被拒绝

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 数据桥接

```
POST /emqx/bridge
```

### Request

- Body
    - **client** `string` 连接客户端 ID
    - **topic** `string` 即时通信主题
    - **payload** `string` 载荷

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}
