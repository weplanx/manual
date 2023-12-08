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

{% swagger method="get" path="" baseUrl="" summary="" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}
