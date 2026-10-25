# DSH Plus · Fake-IP 抓取

[English](README.md) | 中文

让 DSH 的 `web_fetch` 在 Clash / mihomo / sing-box 开启 TUN fake-IP 时正常工作。

属于 [DSH Plus](https://github.com/ArakawaHenri/dshplus) 系列插件（`@dshplus/fake-ip`）。

## 安装

在 DSH 插件管理器中安装 `@dshplus/fake-ip`（或直接安装 [`@dshplus/bundle`](https://github.com/ArakawaHenri/dshplus) 总包）：

```text
@dshplus/fake-ip
```

## 原理与模式

开启 TUN fake-IP 后，DNS 会把域名解析到 `198.18.0.0/15` 保留网段，被 DSH 默认的公网地址校验当作非公网地址拦截（`WEB_BLOCKED_URL`）。

本插件接管了默认的 `http` 抓取器，提供两种模式：
- **`fake-ip` 模式（默认）**：放行 `198.18.0.0/15`（或自定义的 `allowCidrs`），同时继续拦截 `localhost` 和局域网地址。
- **`off` 模式**：完全关闭非公网地址拦截，适合需要让模型直接抓取本地或内网服务的场景。

## 配置

常规 IPv4 fake-IP 环境无需配置。如果开启了 IPv6 fake-IP（例如 `fc00::/18`）或需要切换到 `off` 模式，可在 `cordis.patch.yml` 中配置：

```yaml
- id: dshplus-fake-ip
  config:
    mode: fake-ip
    allowCidrs:
      - 198.18.0.0/15
      - fc00::/18
```

| 字段 | 默认值 | 说明 |
| --- | --- | --- |
| `mode` | `fake-ip` | `fake-ip` 放行指定网段；`off` 允许访问所有内网与回环地址 |
| `allowCidrs` | `['198.18.0.0/15']` | `fake-ip` 模式下放行的 IPv4 / IPv6 CIDR |
| `maxResponseBytes` | `5000000` | 单次响应最大字节数 |
| `maxBodyChars` | `100000` | 返回内容最大字符数 |
| `timeoutMs` | `30000` | 请求超时时间（毫秒） |
| `maxRedirects` | `5` | 同源最大重定向次数 |
| `userAgent` | 原生默认 UA | 自定义请求 `User-Agent` |

## 许可

MIT
