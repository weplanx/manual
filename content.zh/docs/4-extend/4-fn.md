---
weight: 4
title: 云函数
---

# 云函数

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/weplanx/fn/release.yml?label=release&style=flat-square)](https://github.com/weplanx/fn/actions/workflows/release.yml)
[![Release](https://img.shields.io/github/v/release/weplanx/fn.svg?style=flat-square&include_prereleases)](https://github.com/weplanx/fn/releases)
[![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/weplanx/fn?style=flat-square)](https://github.com/weplanx/fn)
[![Go Report Card](https://goreportcard.com/badge/github.com/weplanx/fn?style=flat-square)](https://goreportcard.com/report/github.com/weplanx/fn)
[![GitHub license](https://img.shields.io/github/license/weplanx/fn?style=flat-square)](https://raw.githubusercontent.com/weplanx/fn/main/LICENSE)

用于对象存储文件处理的数据工作流无服务器功能，主要的容器镜像有：

- ghcr.io/weplanx/fn:latest
- registry.cn-shenzhen.aliyuncs.com/weplanx/fn:latest
- ccr.ccs.tencentyun.com/weplanx/fn:latest

## 环境变量

### ADDRESS

- 监听地址，默认 `:9000`

### PROCESS <font color="red">*必须</font>

- 处理逻辑
  - **tencent-cos-excel** 腾讯云 COS 表格处理

### COS_URL

- 腾讯云 COS 地址

### COS_SECRETID

- 腾讯云 COS Secret ID

### COS_SECRETKEY

- 腾讯云 COS Secret Key

## 示例

以腾讯云，云函数为例：

- 选择使用容器镜像，<font color="red">*工作流方式必须为函数服务需要在 COS 命名空间创建</font>
- 函数类型：事件函数
- 函数名称：自定
- 镜像：ccr.ccs.tencentyun.com/weplanx/fn:[version]
- 镜像类型：Web Server 镜像
- 内存：按情况需求
- 环境变量：
  - `PROCESS=tencent-cos-excel`
  - 配置私有的 `COS_URL` `COS_SECRETID` `COS_SECRETKEY`
- 执行配置：<font color="red">*工作流方式必须启用异步执行、状态追踪</font> 

![](/images/extend/fn.png)

### 方式一：COS 触发

部署完成后，至该函数的【触发管理】

- 触发方式：COS 触发
- COS Bucket：选择对应 Bucket
- 事件类型：全部创建
- 后缀过滤：`.excel`

![](/images/extend/fn-trigger.png)

### 方式二：工作流

适合多重处理需自定回调的复杂场景，到对应的 COS 存储桶中，先确认【数据处理】->【媒体处理】是否开启（需要开启）

![](/images/extend/fn-cos-ci.png)

再跳转至【任务与工作流】->【工作流管理】，创建工作流

- 工作流名称：自定
- 匹配规则：自定义规则
  - 自定义后缀：excel
- 增加一个自定义函数输入，选择刚才建立的云函数，如图
- 回调方式：启用并自定义处理事件

![](/images/extend/workflow.png)

创建完成后启用上传触发执行

# 客户端

用于应用程序执行相关云函数

```shell
go get github.com/weplanx/fn
```

## 初始化

```golang
// Create the fn client
x, err = fn.New(
    fn.SetCos(values.Cos.Url, values.Cos.SecretId, values.Cos.SecretKey),
)
```

## 向 COS 上传 Excel 编码并异步转换

```golang
data := [][]interface{}{
    {"Name", "CCType", "CCNumber", "Century", "Currency", "Date", "Email", "URL"},
}
for n := 0; n < 10; n++ {
    data = append(data, []interface{}{
        faker.Name(), faker.CCType(), faker.CCNumber(), faker.Century(), faker.Currency(), faker.Date(), faker.Email(), faker.URL(),
    })
}
ctx := context.TODO()
err := x.TencentCosExcel(ctx, "test", map[string][][]interface{}{
    "Sheet1": data,
})
```


# License

[BSD-3-Clause License](https://github.com/weplanx/fn/blob/main/LICENSE)
