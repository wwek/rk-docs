---
title: "限流拦截器"
linkTitle: "限流拦截器"
weight: 11
description: >
  启动限流拦截器。
---

## 安装
```shell script
go get github.com/rookie-ninja/rk-boot
```

## 通用选项
> 启动器包含了如下通用选项，这些选项是启动 GRPC 服务的必要选项。

| 名字 | 描述 | 类型 | 默认值 | 必要与否
| ------ | ------ | ------ | ------ | ------ |
| grpc.name | GRPC 服务名称 | string | "", 服务不会启动 | Required |
| grpc.port | GRPC 服务端口 | integer | 0, 服务不会启动 | Required |
| grpc.enabled | GRPC 服务启动开关 ｜ bool | false | Required |
| grpc.description | GRPC 服务的描述 | string | "" | Optional |

## 限流选项
| 名字 | 描述 | 类型 | 默认值 |
| ------ | ------ | ------ | ------ |
| grpc.interceptors.rateLimit.enabled | 启动限流兰姐去 | boolean | false |
| grpc.interceptors.rateLimit.algorithm | 限流算法， 支持 tokenBucket 和 leakyBucket | string | tokenBucket |
| grpc.interceptors.rateLimit.reqPerSec | 全局限流值 | int | 0 |
| grpc.interceptors.rateLimit.paths.path | gRPC 方法路径 | string | "" |
| grpc.interceptors.rateLimit.paths.reqPerSec | 基于 gRPC 方法路径的限流值 | int | 0 |

## 快速开始
### 1.创建 boot.yaml
```yaml
---
grpc:
  - name: greeter
    port: 8080
    enabled: true
    commonService:
      enabled: true          # Enable common service for testing
    interceptors:
      rateLimit:
        enabled: false
        algorithm: "leakyBucket"
        reqPerSec: 0
        paths:
          - path: "/rk.api.v1.RkCommonService/Healthy"
            reqPerSec: 0
```

### 2.创建 main.go
```go
package main

import (
	"context"
	"github.com/rookie-ninja/rk-boot"
)

// Application entrance.
func main() {
	// Create a new boot instance.
	boot := rkboot.NewBoot()

	// Bootstrap
	boot.Bootstrap(context.Background())

	// Wait for shutdown sig
	boot.WaitForShutdownSig(context.Background())
}
```

### 3.验证
> 发送请求

```shell script
$ grpcurl -plaintext localhost:1949 rk.api.v1.RkCommonService.Healthy
Error invoking method "rk.api.v1.RkCommonService.Healthy": rpc error: code = ResourceExhausted desc = failed to query for service descriptor "rk.api.v1.RkCommonService": Slow down your request
```

```shell script
$ curl -X GET localhost:8080/rk/v1/healthy
{
    "error":{
        "code":429,
        "status":"Too Many Requests",
        "message":"Slow down your request.",
        "details":[
            {
                "code":8,
                "status":"ResourceExhausted",
                "message":"[from-grpc] Slow down your request."
            }
        ]
    }
}
```

### _**Cheers**_
![](/bootstrapper/user-guide/cheers.png)


