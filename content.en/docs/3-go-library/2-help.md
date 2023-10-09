---
weight: 2
title: Helper
---

# Helper

## Pointer

```golang
x1 := help.Ptr[string]("hello")
x2 := help.Ptr[int64](123)
x3 := help.Ptr[bool](false)
```

## Empty

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

## Validator

Add `help.Validator()` to Hertz configuration

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

## Unified Error

```golang
// help.E("<Business Code>","Msg")
help.E(
    "lark.VerificationTokenNotMatch",
    "the local configuration token does not match the authentication token",
)
```

## Random

```golang
v1 := help.Random(16) // WEpFXPzZDIOX0qBg
v2 := help.Random(32) // HbsMypSzy6obPQQnBRbGIWGtOjPS7s9P
v3 := help.RandomNumber(6) // 614519
v4 := help.RandomAlphabet(16) // UHsRzcKPPEEFqiXJ
v5 := help.RandomUppercase(8) // BLMBHTMJ
v6 := help.RandomLowercase(8) // efcbxrbo
```

## Reverse

```golang
v1 := []string{"a", "b", "c"}
help.Reverse(v) // []string{"c", "b", "a"}

help.ReverseString("abcdefg") // gfedcba
```

## Shuffle

```golang
help.Shuffle([]int{1, 2, 3, 4, 5, 6, 7}) // []int{6, 5, 4, 2, 7, 3, 1}
help.ShuffleString("abcdefg") // afbdgce
```
