---
weight: 3
title: Sessions
---

# Sessions

## Get

```
GET /sessions
```

### Response

{{<hint info>}}

200 OK

- Body `[]string` User IDs

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

{{<tabs `get`>}}
{{<tab `Simple`>}}

```http
GET /sessions HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

["64bfbe69eddf04482dcf9d43","64ae547e5a94adff829274c7"]
```

{{</tab>}}
{{</tabs>}}

## Interrupt

```
DELETE /sessions/:uid
```

### Request

- Path
    - **uid** <font color="red">*required</font> `string` User ID

### Response

{{<hint info>}}

200 OK

- Body
    - **DeletedCount**

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

{{<tabs `delete`>}}
{{<tab `Simple`>}}

```http
DELETE /sessions/64bfbe69eddf04482dcf9d43 HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":1}
```

{{</tab>}}
{{</tabs>}}

## Interrupt All

```
POST /sessions/clear
```

### Response

{{<hint info>}}

200 OK

- Body
  - **DeletedCount**

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

{{<tabs `clear`>}}
{{<tab `Simple`>}}

```http
POST /sessions/clear HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":2}
```

{{</tab>}}
{{</tabs>}}

