---
weight: 6
title: Collaboration
---

# Feishu & Lark

## Verify

```
POST /lark
```

### Request

- Body
    - **encrypt** <font color="red">*required</font> `string` Encrypted data

### Response

{{<hint info>}}

200 OK

- Body
  - **challenge** `string`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## OAuth2

```
GET /lark
```

### Request

- Query
    - **code** <font color="red">*required</font> `string` User login pre-authorization code
    - **state** `object` Custom context
      - **action** `string` custom action
      - **locale** `string` locale

### Response

{{<hint info>}}

302 Found

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

401 Unauthorized

- Body
  - **code** `string` Bussiness code 
  - **message** `string`

{{</hint>}}
