---
weight: 1
title: Common
---

# Common

## Ping

```
GET /
```

### Response

{{<hint info>}}

200 OK (Access to Cookies XSRF_TOKEN)

- Body
    - **name** `string` Host, pod name
    - **ip** `string` Real IP
    - **now** `date` Current time
    - **values** `object` The node is runtime configured and returned when Mode != release

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Login

```
POST /login
```

### Request

- Body
    - **email** <font color="red">*required</font> `string`
    - **password** <font color="red">*required</font> `string`

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## SMS Login Code

```
GET /login/sms
```

### Response

{{<hint info>}}

 204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## SMS Login

```
POST /login/sms
```

### Request

- Body
    - **phone** <font color="red">*required</font> `string`
    - **code** <font color="red">*required</font> `string` sms login code

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## TOTP Login

```
POST /login/totp
```

### Request

- Body
    - **email** <font color="red">*required</font> `string`
    - **code** <font color="red">*required</font> `string` totp code

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Reset Password Code

```
GET /forget_code
```

### Request

- Query
  - **email** <font color="red">*required</font> `string`

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

## Reset Password

```
POST /forget_reset
```

### Request

- Body
  - **email** <font color="red">*required</font> `string`
  - **code** <font color="red">*required</font> `string` Reset password code
  - **password** <font color="red">*required</font> `string` Reset password

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
  - **message** `string`

{{</hint>}}

## Verify

```
GET /verify
```

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

401 Unauthorized

- Body
    - **code** `string` Business code
    - **message** `string`

{{</hint>}}

## Token Refresh Code

```
GET /refresh_code
```

### Response

{{<hint info>}}

200 OK

- Body
    - **code** `string`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Refresh Token

```
POST /refresh_token
```

### Request

- Body
    - **code** <font color="red">*required</font> `string` Token refresh code

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Logout

```
POST /logout
```

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

## User Info

```
GET /user
```

### Response

{{<hint info>}}

200 OK

- Body
    - **_id** `string` User ID
    - **email** `string` User email
    - **name** `string` User name
    - **avatar** `string` User avatar
    - **phone** `string` Phone settings status
    - **sessions** `number` Total number of sessions
    - **history** `object` Recent login history
    - **totp** `string` TOTP settings status
    - **lark** `object` Lark info
    - **status** `bool` User status
    - **create_time** `date`
    - **update_time** `date`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Set User

```
PATCH /user
```

### Request

- Body
    - **key** <font color="red">*required</font> `Email|Name|Avatar` Specify updated field
    - **email** `string` RequiredIf: key=Email
    - **name** `string` RequiredIf: key=Name
    - **avatar** `string` RequiredIf: key=Avatar

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Set Password

```
POST /user/password
```

### Request

- Body
    - **old** <font color="red">*required</font> `string` Old password
    - **password** <font color="red">*required</font> `string` New password

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## User SMS Code

```
GET /user/phone_code
```

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Bind User's Phone

```
POST /user/phone
```

### Request

- Body
    - **phone** <font color="red">*required</font> `string` Phone number
    - **code** <font color="red">*required</font> `string` SMS code

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Generate TOTP URL

```
GET /user/totp
```

### Response

{{<hint info>}}

200 OK

- Body
    - **totp** `string` TOTP URL

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Bind TOTP

```
POST /user/totp
```

### Request

- Body
    - **totp** `string` TOTP URL
    - **tss** `string[]` Two consecutive code

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Unset User

```
DELETE /user/:key
```

### Request

- Path
    - **key** `phone|totp|lark` Specify updated field

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Load Option

```
GET /options
```

### Request

- Query
    - **type** `string` Type, include: upload、collaboration、generate-secret

### Response

{{<hint info>}}

200 OK

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}
