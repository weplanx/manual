---
weight: 1
title: EMQX HTTP
---

# EMQX HTTP

## Authentication

```
POST /emqx/auth
```

### Request

- Body
  - **identity** `string` MongoId of IM authorization project
  - **token** `string` Token issued by the project to which it belongs

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Authorization

```
POST /emqx/acl
```

### Request

- Body
    - **identity** `string` MongoId of IM authorization project
    - **topic** `string`

### Response

{{<hint info>}}

Allow - 204 No Content

{{</hint>}}

{{<hint danger>}}

Deny - 200 OK

- Body
    - **result** `string` = "deny" is rejected

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## Data Bridging

```
POST /emqx/bridge
```

### Request

- Body
    - **client** `string` Client ID
    - **topic** `string`
    - **payload** `string`

### Response

{{<hint info>}}

204 No Content

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}
