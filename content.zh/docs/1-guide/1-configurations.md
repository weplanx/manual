---
weight: 1
title: 配置项
---

# 配置项

应用服务的配置项由环境变量和分布动态配置组成，每个配置项都会赋值到后端服务的变量 Values
中，另外当动态配置发布时每个同域节点（即同租户下）也将自动同步变量 Values 使其动态化

![](/images/configurations/init.png)

## 环境变量

### MODE

- 工作模式，默认 `debug`

### HOSTNAME

- 主机，默认是 pod 的 hostname

### ADDRESS

- 监听地址，默认 `:3000`
- XAPI 默认 `:6000`，OpenAPI 默认 `:9000`

### CONSOLE <font color="red">*必须</font>

- 前端的公开地址
- 例如：https://console.developer.com

### IP

- 自定义获取 IP 的头，默认 `X-Forwarded-For`
- 例如：经过 CDN、WAF 等回源 Kubernetes 将覆盖原 `X-Forwarded-For` 失去真实 IP

### XDOMAIN

- 自定义 XSRF Cookie 域名
- 例如：解决前后端分离导致的 XSRF Token 无法获取的问题

### NAMESPACE <font color="red">*必须</font>

- 应用命名空间，在同个 Nats 租户中是唯一的，DevOps 默认是 weplanx 租户

### KEY <font color="red">*必须</font>

- 应用密钥，用于签名、数据加密等

### DATABASE_URL <font color="red">*必须</font>

- MongoDB 的连接地址

### DATABASE_NAME <font color="red">*必须</font>

- MongoDB 的数据库名称

### DATABASE_REDIS <font color="red">*必须</font>

- Redis 的连接地址

### NATS_HOSTS <font color="red">*必须</font>

- Nats 的连接主机，多台使用`,`分割

### NKEY_NKEY <font color="red">*必须</font>

- Nats 的 NKEY 鉴权密钥

### INFLUX_URL <font color="red">*必须</font>

- InfluxDB 的连接地址

### INFLUX_ORG <font color="red">*必须</font>

- InfluxDB 的组织命名

### INFLUX_TOKEN <font color="red">*必须</font>

- InfluxDB 的授权 Token

### INFLUX_BUCKET <font color="red">*必须</font>

- InfluxDB 存储桶

### OTLP_ENDPOINT <font color="red">*必须</font>

- OpenTelemetry 的 ENDPOINT

### OTLP_TOKEN <font color="red">*必须</font>

- OpenTelemetry 的 TOKEN

## 分布动态配置

动态配置基于 Nats KeyValue 实现，是支持分布式应用配置同步与分发的核心。
数据存储均是加密的，但在鉴权后允许解密返回，<font color="red">*脱敏</font>代表该部分字段不会完整返回，当返回`*`时说明存在值。

![](/images/configurations/sync.png)

{{< hint info >}}

- 管理后台【设置】->【动态配置】中可进行可视化设置
- 定义配置文件 *default.values.yml*，配置名对应 **Snake Case** 即可
  {{< /hint >}}

### SessionTTL

- `time.Duration` 会话周期，默认 `1h`
- 例如：用户活跃操作中会话将续约 n 秒

### LoginTTL

- `time.Duration` 登录失败锁定周期，默认 `15m`

### LoginFailures

- `int64` 连续登录失败的最大次数，默认 `5`

### IpLoginFailures

- `int64` 同 IP 连续登录失败的最大次数，默认 `10`

### IpWhitelist

- `[]string` IP 白名单

### IpBlacklist

- `[]string` IP 黑名单

### PwdStrategy

- `int` 密码强度策略
    - `0` 无限制
    - `1` 小写+大写字母组合
    - `2` 小写+大写字母+数字组合
    - `3` 小写+大写字母+数字+特殊符号组合

### PwdTTL

- `time.Duration` 密码有效周期，`0` 永久

### Cloud

- `string` 公有云平台
    - `tencent` 腾讯云
    - `aliyun` 阿里云
    - `aws` 亚马逊云

### TencentSecretId

- `string` 腾讯云的 Secret ID
- 建议为本应用创建 CAM 子用户并授权所需的最小化权限

### TencentSecretKey <font color="red">*脱敏</font>

- `string` 腾讯云的 Secret Key

### TencentCosBucket

- `string` 腾讯云 COS 的桶

### TencentCosRegion

- `string` 腾讯云 COS 的地域

### TencentCosExpired

- `int64` 腾讯云 COS 上传预签名有效期，单位秒

### TencentCosLimit

- `int64` 腾讯云 COS 上传大小限制

### Collaboration

- `string` 办公协作平台
    - `lark` Lark 或飞书

### LarkAppId

- `string` Lark & 飞书的 App ID

### LarkAppSecret <font color="red">*脱敏</font>

- `string` Lark & 飞书的 App Secret

### LarkEncryptKey <font color="red">*脱敏</font>

- `string` Lark & 飞书的 Encrypt Key

### LarkVerificationToken <font color="red">*脱敏</font>

- `string` Lark & 飞书的 Verification Token

### RedirectUrl

- `string` 第三方重定向地址

### EmailHost

- `string` 公共邮箱 SMTP 地址

### EmailPort

- `int` 公共邮箱 SMTP 端口号（SSL），默认 `465`

### EmailUsername

- `string` 公共邮箱用户

### EmailPassword <font color="red">*脱敏</font>

- `string` 公共邮箱 SMTP 密码

### ApiGatewayUrl

- `string` API 网关地址

### ApiGatewayKey

- `string` API 网关应用的 Key
- 例如：腾讯云API网关[应用认证方式](https://cloud.tencent.com/document/product/628/55088)

### ApiGatewaySecret <font color="red">*脱敏</font>

- `string` API 网关应用的 Secret

### RestControls

- `map[string]*RestControl` 数据集动态配置
- RestControl
    - **Keys** `[]string` 投影字段
    - **Sensitives** `[]string` 脱敏字段
    - **Status** `bool` 是否允许访问
    - **Event** `bool` 是否启用事件回调

### RestTxnTimeout

- `time.Duration` 事务挂起有效期，默认 `3m`

## 扩展动态配置

weplanx/server 还扩展了其他配置，如果是定制其他应用是不需要的：

### IpAddress

- `string` 获取 IPv4 详情的地址接口
- 目前使用腾讯云市场[数链云](https://market.cloud.tencent.com/products/38618)
    - 日志集中标记为 `metadata.version: shuliancloud.v4`

### IpSecretId

- `string` 获取 IPv4 详情的 Secret ID

### IpSecretKey <font color="red">*脱敏</font>

- `string` 获取 IPv4 详情的 Secret Key

### Ipv6Address

- `string` 获取 IPv6 详情的地址接口
- 目前使用腾讯云市场[数链云](https://market.cloud.tencent.com/products/38620)
    - 日志集中标记为 `metadata.version: shuliancloud.v4`

### Ipv6SecretId

- `string` 获取 IPv6 详情的 Secret ID

### Ipv6SecretKey <font color="red">*脱敏</font>

- `string` 获取 IPv6 详情的 Secret Key

### SmsSecretId

- `string` 腾讯云[短信](https://cloud.tencent.com/document/product/382/37745)的 Secret ID

### SmsSecretKey <font color="red">*脱敏</font>

- `string` 腾讯云短信的 Secret

### SmsSign

- `string` 腾讯云短信签名

### SmsAppId

- `string` 腾讯云短信应用 ID

### SmsRegion

- `string` 腾讯云短信地域

### SmsPhoneBind

- `string` 腾讯云短信手机绑定模板 ID

### SmsLoginVerify

- `string` 腾讯云短信登录模版 ID

### EmqxHost

- `string` [EMQX API](https://www.emqx.io/docs/zh/v5.1/admin/api-docs.html) 主机地址

### EmqxApiKey

- `string` EMQX API 的 Key

### EmqxSecretKey <font color="red">*脱敏</font>

- `string` EMQX API 的 Secret Key

### AccelerateAddress

- `string` 腾讯云资源加速云函数回调地址

### CamUin

- `string` 腾讯云主账号 ID

