---
weight: 2
title: 助手函数
---

# 助手函数

## 转指针

```golang
x1 := help.Ptr[string]("hello")
x2 := help.Ptr[int64](123)
x3 := help.Ptr[bool](false)
```

## 是否为空

```golang
help.IsEmpty(nil) // true
help.IsEmpty("") // true
help.IsEmpty(0) // true
help.IsEmpty(false) // true
help.IsEmpty([]string{}) // true
help.IsEmpty(help.Ptr[int64](0)) // false

var a *string
help.IsEmpty(a) // true

var b struct{}
help.IsEmpty(b) // true
```

## 接入验证器

在 Hertz 配置中增加 `help.Validator()`

```golang {hl_lines=[5]}
func UseHertz(v *common.Values) (h *server.Hertz, err error) {
	tracer, cfg := tracing.NewServerTracer()
	opts := []config.Option{
		server.WithHostPorts(v.Address),
		server.WithCustomValidator(help.Validator()),
		tracer,
	}
	if os.Getenv("MODE") != "release" {
		opts = append(opts, server.WithExitWaitTime(0))
	}
	opts = append(opts)
	h = server.Default(opts...)
	h.Use(
		requestid.New(),
		help.EHandler(),
		tracing.ServerMiddleware(cfg),
	)
	return
}
```

## 统一错误

```golang
// help.E("<业务错误码>","错误信息")
help.E(
    "lark.VerificationTokenNotMatch",
    "the local configuration token does not match the authentication token",
)
```

## 随机生成

```golang
v1 := help.Random(16) // WEpFXPzZDIOX0qBg
v2 := help.Random(32) // HbsMypSzy6obPQQnBRbGIWGtOjPS7s9P
v3 := help.RandomNumber(6) // 614519
v4 := help.RandomAlphabet(16) // UHsRzcKPPEEFqiXJ
v5 := help.RandomUppercase(8) // BLMBHTMJ
v6 := help.RandomLowercase(8) // efcbxrbo
```

## 反转

```golang
v1 := []string{"a", "b", "c"}
help.Reverse(v) // []string{"c", "b", "a"}

help.ReverseString("abcdefg") // gfedcba
```

## 打乱

```golang
help.Shuffle([]int{1, 2, 3, 4, 5, 6, 7}) // []int{6, 5, 4, 2, 7, 3, 1}
help.ShuffleString("abcdefg") // afbdgce
```
