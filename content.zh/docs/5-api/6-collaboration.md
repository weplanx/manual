---
weight: 6
title: 办公协作
---

# 飞书 & Lark

## 验证地址

```
POST /lark
```

### Request

- Body
    - **encrypt** <font color="red">*必须</font> `string` 加密数据

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body
  - **challenge** `string` 验证值

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## OAuth2

```
GET /lark
```

### Request

- Query
    - **code** <font color="red">*必须</font> `string` 用户登录预授权码
    - **state** `object` 自定义上下文
      - **action** `string` 操作
      - **locale** `string` 语言

### Response

{{<hint info>}}

重定向 - 302 Found

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

授权失败 - 401 Unauthorized

- Body
  - **code** `string` 业务码 
  - **message** `string` 错误消息

{{</hint>}}
