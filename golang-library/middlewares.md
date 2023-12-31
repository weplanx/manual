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

# Middlewares

## CSRF

Add in the api.go of the project program

```go
func (x *API) Routes(h *server.Hertz) (err error) {
	csrfToken := x.Csrf.VerifyToken(!x.V.IsRelease())

	...
	h.GET("", x.Index.Ping)
	_login := h.Group("login", csrfToken)
	{
	    ...
	}
	...
}
```

To separate single page front-end loading XSRF\_TOKEN need to be added in `Index.Ping`

```go
func (x *Controller) Ping(_ context.Context, c *app.RequestContext) {
	x.Csrf.SetToken(c)
	r := M{
		"name": x.V.Hostname,
		"ip":   string(c.GetHeader(x.V.Ip)),
		"now":  time.Now(),
	}
	if !x.V.IsRelease() {
		r["values"] = x.V
	}
	c.JSON(200, r)
}
```

For further security, the front and back-end domain names need to be of the same origin, and the cookie SameSite is set to `Strict`

## Error Chain

Set up global error chain in Hertz middleware

```go
func UseHertz(v *common.Values) (h *server.Hertz, err error) {
    ...
	h = server.Default(opts...)
	h.Use(
		requestid.New(),
		help.EHandler(),
		tracing.ServerMiddleware(cfg),
	)
	return
}
```
