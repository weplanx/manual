---
weight: 3
title: 中间件
---

# 中间件

## CSRF 保护

在项目程序的 api.go 文件中新增

```golang {hl_lines=[2,5]}
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

为分离单页面前端载入 XSRF_TOKEN 需在 `Index.Ping` 增加

```golang {hl_lines=[2]}
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

为进一步保障安全，前后端域名需要同源，Cookie SameSite 设置为 `Strict`

## 错误链返回

在 Hertz 中间件设置全局使用错误链返回

```golang {hl_lines=[6]}
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
