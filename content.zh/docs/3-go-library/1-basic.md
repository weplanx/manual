---
weight: 1
title: 基本功能
---

# 基本功能

在应用程序中安装

```shell
go get github.com/weplanx/go
```

## 认证鉴权

```golang
// Create passport
x1 = passport.New(
    passport.SetIssuer("dev"),
    passport.SetKey(key1),
)

// Create token
token, err = x1.Create(userId1, jti1, time.Hour*2)

// Verify token
clamis1, err = x1.Verify(token)
```

## 密码

```golang
// Generate hash value
hash, err := passlib.Hash("pass@VAN1234")

// Verify password
err = passlib.Verify("pass@VAN1234", hash)
```

## 动态口令

```golang
// Authenticate
x := &totp.Totp{
    Secret:       "2SH3V3GDW7ZNMGYE",
    Window:       3,
    Counter:      1,
    ScratchCodes: []int{11112222, 22223333},
}

values := []Value1{
    {"foobar", false},
    {"1fooba", false},
    {"1111111", false},
    {"293240", true},
    {"293240", false},
    {"33334444", false},
    {"11112222", true},
    {"11112222", false},
}

for _, v := range values {
    r, _ := x.Authenticate(v.code)
}

```

## 数据加密

```golang
// Create cipher
x1, err = cipher.New("6ixSiEXaqxsJTozbnxQ76CWdZXB2JazK")

var text = "Gophers, gophers, gophers everywhere!"
var encryptedText string

// Encode
encryptedText, err = x1.Encode([]byte(text))

// Decode
decryptedText, err := x1.Decode(encryptedText)
```

## 验证码

```golang
// Create captcha
x = captcha.New(
    captcha.SetNamespace(namespace),
    captcha.SetRedis(redis.NewClient(opts)),
)

// Create code
ctx := context.TODO()
status := x.Create(ctx, "dev1", "abcd", time.Second*60)

// Verify
err = x.Verify(context.TODO(), "dev1", "abc")

// Exists
exists := x.Exists(context.TODO(), "dev1")

// Delete
result := x.Delete(context.TODO(), "dev1")
```

## 验证锁

```golang
// Create locker
x = locker.New(
    locker.SetNamespace(namespace),
    locker.SetRedis(redis.NewClient(opts)),
)

// Update
ctx := context.TODO()
n := x.Update(ctx, "dev", time.Second*60)

// Verify
err := x.Verify(ctx, "dev", 3)

// Delete
result := x.Delete(ctx, "dev")
```
