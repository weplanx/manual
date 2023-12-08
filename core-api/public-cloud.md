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

# Public Cloud

{% swagger method="get" path="" baseUrl="/tencent/cos_presigned" summary="COS Pre-Signed" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```
- Body [More](https://cloud.tencent.com/document/product/436/14690)
    - **key** `string` Generate object
    - **policy** `string`
    - **q-ak** `string`
    - **q-key-time** `string`
    - **q-sign-algorithm** `string`
    - **q-signature** `string`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/tencent/cos_image_info" summary="COS Image Info" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="url" type="String" required="true" %}
object
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```
- Body [More](https://cloud.tencent.com/document/product/460/6927)
    - **bit_depth** `string`
    - **format** `string`
    - **frame_count** `string`
    - **height** `string`
    - **horizontal_dpi** `string`
    - **md5** `string`
    - **size** `string`
    - **vertical_dpi** `string`
    - **width** `string`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}
