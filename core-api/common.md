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

# Common

{% swagger method="get" path="" baseUrl="/" summary="Ping" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="Access to Cookies XSRF_TOKEN" %}
```
- Body
    - **name** `string` Host, pod name
    - **ip** `string` Real IP
    - **now** `date` Current time
    - **values** `object` The node is runtime configured and returned when Mode != release
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/login" summary="Login" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="email" required="true" type="String" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="password" type="String" required="true" %}

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

{% swagger method="get" path="" baseUrl="/login/sms" summary="SMS Login Code" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="204: No Content" description="" %}

{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/login/sms" summary="SMS Login" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="phone" type="String" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="code" type="String" required="true" %}
sms login code
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

{% swagger method="post" path="" baseUrl="/login/totp" summary="TOTP Login" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="email" type="String" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="code" type="String" required="true" %}
Totp Code
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

{% swagger method="get" path="" baseUrl="/forget_code" summary="Reset Password Code" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="email" type="String" required="true" %}

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

{% swagger method="post" path="" baseUrl="/forget_reset" summary="Reset Password" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="email" type="String" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="code" type="String" required="true" %}
Reset password code
{% endswagger-parameter %}

{% swagger-parameter in="body" name="password" type="String" required="true" %}
Reset password
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

{% swagger method="get" path="" baseUrl="/verify" summary="Verify" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="204: No Content" description="" %}

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
    - **code** `string` Business code
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/refresh_code" summary="Token Refresh Code" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```
- Body
    - **code** `string`
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/refresh_token" summary="Refresh Token" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="code" type="String" required="true" %}
Token refresh code
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

{% swagger method="post" path="" baseUrl="/logout" summary="Logout" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="204: No Content" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/user" summary="User Info" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```
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
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="patch" path="" baseUrl="/user" summary="Set User" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="key" required="true" type="String" %}
`Email|Name|Avatar` Specify updated field
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email" type="String" %}
RequiredIf: key=Email
{% endswagger-parameter %}

{% swagger-parameter in="body" name="name" type="String" %}
RequiredIf: key=Name
{% endswagger-parameter %}

{% swagger-parameter in="body" name="avatar" type="String" %}
RequiredIf: key=Avatar
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

{% swagger method="post" path="" baseUrl="/user/password" summary="Set Password" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="old" type="String" required="true" %}
Old password
{% endswagger-parameter %}

{% swagger-parameter in="body" name="password" type="String" required="true" %}
New password
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

{% swagger method="get" path="" baseUrl="/user/phone_code" summary="User SMS Code" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="204: No Content" description="" %}

{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/user/phone" summary="Bind User's Phone" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="phone" required="true" type="String" %}
Phone number
{% endswagger-parameter %}

{% swagger-parameter in="body" name="code" required="true" type="String" %}
SMS code
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

{% swagger method="get" path="" baseUrl="/user/totp" summary="Generate TOTP URL" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
```
- Body
    - **totp** `string` TOTP URL
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/user/totp" summary="Bind TOTP" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="body" name="totp" type="String" required="true" %}
TOTP URL
{% endswagger-parameter %}

{% swagger-parameter in="body" name="tss" type="String[]" required="true" %}
Two consecutive code
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

{% swagger method="delete" path="" baseUrl="/user/:key" summary="Unset User" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="key" type="String" required="true" %}
`phone|totp|lark` Specify updated field
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

{% swagger method="get" path="" baseUrl="/options" summary="Load Option" expanded="true" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="type" type="String" required="true" %}
Type, include: upload、collaboration、generate-secret
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}

{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```
- Body
    - **message** `string`
```
{% endswagger-response %}
{% endswagger %}
