---
title: "高级指南"
linkTitle: "高级指南"
weight: 2
description: >
  通过高级指南，用户可以学习如何通过修改 boot.yaml 文件，对服务进行高级配置。
---

## 概述
我们将会在服务中启动如下功能：

| 功能 | 详情 |
| ---- | ---- |
| 环境区分 | 如何根据环境变量或许不同的配置文件？|
| 日志 | 用户自定义日志 |
| TLS/SSL 认证 | 启动 TLS/SSL |
| 配置文件 | 如何使用 [Viper](https://github.com/spf13/viper) 进行配置文件的读取操作？ |
| 应用信息 | 配置用户自定义的应用信息。|
| 启动多个服务 | 一个进程中启动多个 Gin 服务。 |
| 正常关闭 | 注册进程关闭钩子 |
| 错误类型 | 标准错误类型 |
| 覆盖启动器参数 | 覆盖启动器参数 |
| 分布式日志追踪 | 通过 traceId 在日志中追踪 RPC | 