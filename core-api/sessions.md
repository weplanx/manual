---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Sessions

{% swagger method="get" path="" baseUrl="/sessions" summary="Get" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```
- Body `[]string` User IDs
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
```http
GET /sessions HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

["64bfbe69eddf04482dcf9d43","64ae547e5a94adff829274c7"]
```
{% endtab %}
{% endtabs %}

{% swagger method="delete" path="" baseUrl="/sessions/:uid" summary="Interrupt" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="uid" type="String" required="true" %}
User ID
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body
    - **DeletedCount**
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
```http
DELETE /sessions/64bfbe69eddf04482dcf9d43 HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":1}
```
{% endtab %}
{% endtabs %}

{% swagger method="post" path="" baseUrl="/sessions/clear" summary="Interrupt All" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```
- Body
  - **DeletedCount**
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Simple" %}
```http
POST /sessions/clear HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"DeletedCount":2}
```
{% endtab %}
{% endtabs %}
