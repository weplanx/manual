---
weight: 5
title: 公有云
---

# 腾讯云

## COS 预签名

```
GET /tencent/cos_presigned
```

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body [查看](https://cloud.tencent.com/document/product/436/14690)
    - **key** `string` 生成对象名称
    - **policy** `string` 策略
    - **q-ak** `string`
    - **q-key-time** `string`
    - **q-sign-algorithm** `string`
    - **q-signature** `string`

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}

## COS 图片信息

```
GET /tencent/cos_image_info
```

### Request

- Query
    - **url** <font color="red">*必须</font> `string` 对象路径

### Response

{{<hint info>}}

返回成功 - 200 OK

- Body [查看](https://cloud.tencent.com/document/product/460/6927)
    - **bit_depth** `string`
    - **format** `string`
    - **frame_count** `string`
    - **height** `string`
    - **horizontal_dpi** `string`
    - **md5** `string`
    - **size** `string`
    - **vertical_dpi** `string`
    - **width** `string`

{{</hint>}}

{{<hint danger>}}

返回失败 - 400 Bad Request

- Body
    - **message** `string` 错误消息

{{</hint>}}
