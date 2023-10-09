---
weight: 2
title: 基础能力
---

# 指令集

## 自动重试

```
img[wpxRetry]
```

- @Input() wpxRetryCount: number 重试次数，默认 `5` 次 
- @Input() wpxDelay: number 重试间隔，默认 `5000` ms

## 表单提交

判断验证状态并更新表单所有 controls （包含树型结构）的状态

```
form[wpxSubmit]
```

- @Output() readonly wpxSubmit: EventEmitter\<any> 提交事件

## 统一上传

载入远程异步获取的上传配置

```
nz-upload[wpxUpload]
```

- @Input() wpxExt?: string 扩展名

# 预定义管道

## 是否为空

```
{{ value_expression | wpxEmpty }}
```

- value `unknown` 任何
- Return `boolean`

## 字符串分割为数组

```
{{ value_expression | wpxSplit: separator }}
```

- value `string` 字符串
- separator `string` 分割符号
- Return `string`

## 数组拼接为字符串

```
{{ value_expression | wpxJoin: separator }}
```

- value `string[]` 字符串数组
- separator `string` 拼接符号
- Return `string`

## JSON 转换对象

```
{{ value_expression | wpxObject }}
```

- value `string` JSON 字符串
- Return `object` 未知对象

## 数组排序

```
{{ value_expression | wpxSort: field : sort }}
```

- value `any[]` 任何对象数组
- field `string` 排序字段
- sort `1 | -1` 升序或降序，默认升序 `1`

## 填充空字符

```
{{ value_expression | wpxBlank: replace }}
```

- value `any` 任何
- replace `string` 替换字符，默认 `-`

## 生成资源路径

```
{{ value_expression | wpxBlank: query : css }}
```

- value `string[]` URL 片段
- query `string` Query 参数，例如：OSS 或 COS 的对象处理
- css `boolean` 是否为样式
