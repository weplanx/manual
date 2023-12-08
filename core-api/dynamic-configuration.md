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

# Dynamic Configuration

{% swagger method="get" path="" baseUrl="/values" summary="Get" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="keys" type="String[]" %}
Configuration key, empty return all
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body `object` Configurations
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
GET /values HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"Cloud":"tencent",...,"TencentSecretKey":"*"}
```
{% endtab %}

{% tab title="Specify Key" %}
```http
GET /values?keys=IpLoginFailures&keys=LoginFailures HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 200 OK
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz

{"IpLoginFailures":10,"LoginFailures":5}
```
{% endtab %}
{% endtabs %}

{% swagger expanded="true" method="patch" path="" baseUrl="/values" summary="Set" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="update" type="Object" required="true" %}
keyValue data
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="" %}

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
PATCH /values HTTP/1.1
Host: api-x.kainonly.com:8443
Content-Type: application/json

{
    "update": {
        "mytest": "hi"
    }
}

# Response

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```
{% endtab %}
{% endtabs %}

{% swagger method="delete" path="" baseUrl="/values/:key" summary="Remove" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="key" type="String" required="true" %}
Configuration key
{% endswagger-parameter %}

{% swagger-response status="204: No Content" description="" %}

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
DELETE /values/mytest HTTP/1.1
Host: api-x.kainonly.com:8443

# Response

HTTP/1.1 204 No Content
Alt-Svc: h3=":8443"; ma=2592000,h3-29=":8443"; ma=2592000
Server: hertz
```
{% endtab %}
{% endtabs %}
