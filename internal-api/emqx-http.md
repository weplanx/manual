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

# EMQX HTTP

{% swagger method="post" path="" baseUrl="/emqx/auth" summary="Authentication" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="identity" type="String" required="true" %}
MongoId of IM authorization project
{% endswagger-parameter %}

{% swagger-parameter in="body" name="token" type="String" required="true" %}
Token issued by the project to which it belongs
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

{% swagger method="post" path="" baseUrl="/emqx/acl" summary="Authorization" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="identity" type="String" required="true" %}
MongoId of IM authorization project
{% endswagger-parameter %}

{% swagger-parameter in="body" name="topic" type="String" required="true" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Deny" %}
```
- Body
    - **result** `string` = "deny" is rejected
```
{% endswagger-response %}

{% swagger-response status="204: No Content" description="Allow" %}

{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/emqx/bridge" summary="Data Bridging" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="client" type="String" required="true" %}
Client ID
{% endswagger-parameter %}

{% swagger-parameter in="body" name="topic" type="String" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="payload" type="String" required="true" %}

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
