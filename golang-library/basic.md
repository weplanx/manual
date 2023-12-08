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

# Basic

Install in the application

```shell
go get github.com/weplanx/go
```

## Passport

```go
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

## Argon2id

```go
// Generate hash value
hash, err := passlib.Hash("pass@VAN1234")

// Verify password
err = passlib.Verify("pass@VAN1234", hash)
```

## Totp

```go
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

## Data Cipher

```go
// Create cipher
x1, err = cipher.New("6ixSiEXaqxsJTozbnxQ76CWdZXB2JazK")

var text = "Gophers, gophers, gophers everywhere!"
var encryptedText string

// Encode
encryptedText, err = x1.Encode([]byte(text))

// Decode
decryptedText, err := x1.Decode(encryptedText)
```

## Captcha

```go
// Create captcha
x = captcha.New(redis.NewClient(opts))

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

## Verify Locker

```go
// Create locker
x = locker.New(redis.NewClient(opts))

// Update
ctx := context.TODO()
n := x.Update(ctx, "dev", time.Second*60)

// Verify
err := x.Verify(ctx, "dev", 3)

// Delete
result := x.Delete(ctx, "dev")
```
