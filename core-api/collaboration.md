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

# Collaboration

{% swagger method="post" path="" baseUrl="/lark" summary="Lark Verify" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="encrypt" type="String" required="true" %}
Encrypted data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body
  - **challenge** `string`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/lark" summary="Lark OAuth2" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="code" type="String" required="true" %}
User login pre-authorization code
{% endswagger-parameter %}

{% swagger-parameter in="query" name="state" type="Object" %}
Custom context
{% endswagger-parameter %}

{% swagger-parameter in="query" name="state.action" type="String" %}
custom action
{% endswagger-parameter %}

{% swagger-parameter in="query" name="state.locale" type="String" %}
locale
{% endswagger-parameter %}

{% swagger-response status="302: Found" description="" %}

{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="" %}
```
- Body
  - **code** `string` Bussiness code 
  - **message** `string`
```
{% endswagger-response %}
{% endswagger %}
