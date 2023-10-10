---
weight: 5
title: Public Cloud
---

# Tencent Cloud

## COS Pre-Signed

```
GET /tencent/cos_presigned
```

### Response

{{<hint info>}}

200 OK

- Body [More](https://cloud.tencent.com/document/product/436/14690)
    - **key** `string` Generate object
    - **policy** `string`
    - **q-ak** `string`
    - **q-key-time** `string`
    - **q-sign-algorithm** `string`
    - **q-signature** `string`

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}

## COS Image Info

```
GET /tencent/cos_image_info
```

### Request

- Query
    - **url** <font color="red">*required</font> `string` object

### Response

{{<hint info>}}

200 OK

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

{{</hint>}}

{{<hint danger>}}

400 Bad Request

- Body
    - **message** `string`

{{</hint>}}
