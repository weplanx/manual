# Basic

## Retry

```
img[wpxRetry]
```

* **@Input() wpxRetryCount: number** number of retries, default `5` times
* **@Input() wpxDelay: number** interval, default `5000` ms

## Form Submit

Determine the validation state and update the state of all controls (including tree structures) of the form

```
form[wpxSubmit]
```

* **@Output() readonly wpxSubmit: EventEmitter\<any>** submit event

## Unified Upload

Load the upload configuration obtained by remote asynchronization

```
nz-upload[wpxUpload]
```

* **@Input() wpxExt?: string** file extension suffix

## Predefined Pipes

### Empty

```
{{ value_expression | wpxEmpty }}
```

* value `unknown`
* Return `boolean`

### Split

```
{{ value_expression | wpxSplit: separator }}
```

* value `string`
* separator `string`
* Return `string`

### Join

```
{{ value_expression | wpxJoin: separator }}
```

* value `string[]`
* separator `string`
* Return `string`

### JSON conversion object

```
{{ value_expression | wpxObject }}
```

* value `string` JSON string
* Return `object`

### Sort

```
{{ value_expression | wpxSort: field : sort }}
```

* value `any[]`
* field `string`
* sort `1 | -1` Asc or Desc, default Asc `1`

### Blank Filling

```
{{ value_expression | wpxBlank: replace }}
```

* value `any`
* replace `string` replace characters, default `-`

### Generate Assets url

```
{{ value_expression | wpxAssets: query : css }}
```

* value `string[]` URL fragments
* query `string` Query parameters, e.g. object handling for OSS or COS
* css `boolean` a style
