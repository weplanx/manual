---
weight: 1
title: 公共服务
---

# 公共服务

## 载入

```
GET /
```

### Response

{{<hint info>}}

返回成功 - 200 OK (获得 Cookie XSRF_TOKEN)

- Body
    - **name** `string` 主机，pod 名
    - **ip** `string` 真实 IP
    - **now** `date` 当前时间
    - **values** `object` 该节点运行时配置，当 Mode!=release 时返回

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 登录

```
POST /login
```

### Request

- Body
    - **email** <font color="red">*必须</font> `string` 电子邮件
    - **password** <font color="red">*必须</font> `string` 密码

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 获取短信登录验证码

```
GET /login/sms
```

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 短信登录

```
POST /login/sms
```

### Request

- Body
    - **phone** <font color="red">*必须</font> `string` 手机号
    - **code** <font color="red">*必须</font> `string` 验证码

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## TOTP 登录

```
POST /login/totp
```

### Request

- Body
    - **email** <font color="red">*必须</font> `string` 电子邮件
    - **code** <font color="red">*必须</font> `string` 验证码

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 获取重置密码验证

```
GET /forget_code
```

### Request

- Query
  - **email** <font color="red">*必须</font> `string` 电子邮件

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

## 重置密码

```
POST /forget_reset
```

### Request

- Body
  - **email** <font color="red">*必须</font> `string` 电子邮件
  - **code** <font color="red">*必须</font> `string` 验证码
  - **password** <font color="red">*必须</font> `string` 重置密码

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
  - **message** `string` 错误消息

{{</hint>}}

## 验证

```
GET /verify
```

### Response

{{<hint info>}}

返回成功 - 204 No Content

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

## 令牌刷新验证码

```
GET /refresh_code
```

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body
    - **code** `string` 验证码

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 刷新令牌

```
POST /refresh_token
```

### Request

- Body
    - **code** <font color="red">*必须</font> `string` 验证码

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 登出

```
POST /logout
```

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

## 获取用户信息

```
GET /user
```

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body
    - **_id** `string` 用户 ID
    - **email** `string` 电子邮件
    - **name** `string` 称呼
    - **avatar** `string` 头像
    - **phone** `string` 手机设置状态
    - **sessions** `number` 会话次数
    - **history** `object` 最近登录历史
    - **totp** `string` TOTP 设置状态
    - **lark** `object` Lark 信息
    - **status** `bool` 用户状态
    - **create_time** `date` 创建时间
    - **update_time** `date` 更新时间

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 设置用户信息

```
PATCH /user
```

### Request

- Body
    - **key** <font color="red">*必须</font> `Email|Name|Avatar` 更新字段
    - **email** `string` 电子邮件，key=Email 时必须
    - **name** `string` 称呼，key=Name 时必须
    - **avatar** `string` 头像，key=Avatar 时必须

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 修改密码

```
POST /user/password
```

### Request

- Body
    - **old** <font color="red">*必须</font> `string` 旧密码
    - **password** <font color="red">*必须</font> `string` 新密码

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 获取用户手机验证码

```
GET /user/phone_code
```

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 绑定用户手机

```
POST /user/phone
```

### Request

- Body
    - **phone** <font color="red">*必须</font> `string` 手机号
    - **code** <font color="red">*必须</font> `string` 验证码

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 生成 TOTP URL

```
GET /user/totp
```

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body
    - **totp** `string` TOTP URL

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 绑定 TOTP

```
POST /user/totp
```

### Request

- Body
    - **totp** `string` TOTP URL
    - **tss** `string[]` 两次连续验证码

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 取消用户信息

```
DELETE /user/:key
```

### Request

- Path
    - **key** `phone|totp|lark` 取消的字段

### Response

{{<hint info>}}

返回成功 - 204 No Content

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## 配置载入

```
GET /options
```

### Request

- Query
    - **type** `string` 类型，允许 upload、collaboration、generate-secret

### Response

{{<hint info>}}

返回成功 - 200 OK

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}
