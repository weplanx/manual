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

# Integration

## Dynamic Configuration

Embed `*values.DynamicValues` in the project `common.Values`.

Similarly, if you want to customize more configurations, you can first define a `common.Extra` embedded `*values.DynamicValues`, and then embed `common.Values` into it.

```go
type Values struct {
    Mode      string `env:"MODE" envDefault:"debug"`
	Hostname  string `env:"HOSTNAME"`
	Address   string `env:"ADDRESS" envDefault:":3000"`
    ...
    *Extra
}

type Extra struct {
    IpAddress             string `yaml:"ip_address"`
	IpSecretId            string `yaml:"ip_secret_id"`
	IpSecretKey           string `yaml:"ip_secret_key" secret:"*"`
    ...
    *values.DynamicValues `yaml:"dynamic_values"`
}
```

{% hint style="info" %}
The tag `secret` means desensitization, and only shows whether state is set in the `/values` return.
{% endhint %}

Add loading in bootstrap.go

```go
func LoadStaticValues(path string) (v *common.Values, err error) {
	v = new(common.Values)
	if err = env.Parse(v); err != nil {
		return
	}
	var b []byte
	if b, err = os.ReadFile(path); err != nil {
		return
	}
	if err = yaml.Unmarshal(b, &v.Extra); err != nil {
		return
	}
	return
}
```

Also define configuration loading

```go
func UseValues(v *common.Values, kv nats.KeyValue, cipher *cipher.Cipher) *values.Service {
	return values.New(
		values.SetKeyValue(kv),
		values.SetCipher(cipher),
		values.SetType(reflect.TypeOf(common.Extra{})),
	)
}
```

Add it in wire.go

```go
func NewAPI(values *common.Values) (*api.API, error) {
    wire.Build(
		...
		UseValues,
		...
		api.Provides,
	)
    return &api.API{}, nil
}
```

Then dependency in api.go and establish routing

```go
var Provides = wire.NewSet(
    ...
    wire.Struct(new(values.Controller), "*"),
    ...
)

type API struct {
    ...
    Values               *values.Controller
    ...
}

func (x *API) Routes(h *server.Hertz) (err error) {
    ...
    _values := h.Group("values", m...)
	{
		_values.GET("", x.Values.Get)
		_values.PATCH("", x.Values.Set)
		_values.DELETE(":key", x.Values.Remove)
	}
	...
}
```

Final execution generates `wire_gen.go`

```shell
wire ./bootstrap
```

## Sessions

First define configuration loading in bootstrap.go

```go
func UseSessions(v *common.Values, rdb *redis.Client) *sessions.Service {
	return sessions.New(
		sessions.SetRedis(rdb),
		sessions.SetDynamicValues(v.DynamicValues),
	)
}
```

Add it in wire.go

```go
func NewAPI(values *common.Values) (*api.API, error) {
    wire.Build(
		...
		UseSessions,
		...
		api.Provides,
	)
    return &api.API{}, nil
}
```

Dependency in api.go and establish routing

```go
var Provides = wire.NewSet(
    ...
    wire.Struct(new(sessions.Controller), "*"),
    ...
)

type API struct {
    ...
    Sessions             *sessions.Controller
    ...
}

func (x *API) Routes(h *server.Hertz) (err error) {
    ...
    _sessions := h.Group("sessions", m...)
	{
		_sessions.GET("", x.Sessions.Lists)
		_sessions.DELETE(":uid", x.Sessions.Remove)
		_sessions.POST("clear", x.Sessions.Clear)
	}
	...
}
```

Final execution generates `wire_gen.go`

```shell
wire ./bootstrap
```

## Mongo REST

First define configuration loading in bootstrap.go

```go
func UseRest(
	v *common.Values,
	mgo *mongo.Client,
	db *mongo.Database,
	rdb *redis.Client,
	js nats.JetStreamContext,
	keyvalue nats.KeyValue,
	xcipher *cipher.Cipher,
) *rest.Service {
	return rest.New(
		rest.SetMongoClient(mgo),
		rest.SetDatabase(db),
		rest.SetRedis(rdb),
		rest.SetJetStream(js),
		rest.SetKeyValue(keyvalue),
		rest.SetDynamicValues(v.DynamicValues),
		rest.SetCipher(xcipher),
	)
}
```

Add it in wire.go

```go
func NewAPI(values *common.Values) (*api.API, error) {
    wire.Build(
		...
		UseRest,
		...
		api.Provides,
	)
    return &api.API{}, nil
}
```

Dependency in api.go and establish routing

```go
var Provides = wire.NewSet(
    ...
    wire.Struct(new(rest.Controller), "*"),
    ...
)

type API struct {
    ...
    Rest                 *rest.Controller
    ...
}

func (x *API) Routes(h *server.Hertz) (err error) {
    ...
    _db := h.Group("db", csrfToken, auth)
	{
		_db.GET(":collection/:id", x.Rest.FindById)
		_db.POST(":collection/create", audit, x.Rest.Create)
		_db.POST(":collection/bulk_create", audit, x.Rest.BulkCreate)
		_db.POST(":collection/size", x.Rest.Size)
		_db.POST(":collection/find", x.Rest.Find)
		_db.POST(":collection/find_one", x.Rest.FindOne)
		_db.POST(":collection/update", audit, x.Rest.Update)
		_db.POST(":collection/bulk_delete", audit, x.Rest.BulkDelete)
		_db.POST(":collection/sort", audit, x.Rest.Sort)
		_db.PATCH(":collection/:id", audit, x.Rest.UpdateById)
		_db.PUT(":collection/:id", audit, x.Rest.Replace)
		_db.DELETE(":collection/:id", audit, x.Rest.Delete)
		_db.POST("transaction", audit, x.Rest.Transaction)
		_db.POST("commit", audit, x.Rest.Commit)
	}
	...
}
```

Final execution generates `wire_gen.go`

```shell
wire ./bootstrap
```
