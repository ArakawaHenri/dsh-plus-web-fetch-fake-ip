# DSH Plus · Fake-IP Fetch

English | [中文](README.zh.md)

Makes DSH's `web_fetch` work smoothly when Clash, mihomo, or sing-box is running in TUN fake-IP mode.

Part of [DSH Plus](https://github.com/ArakawaHenri/dshplus) (`@dshplus/fake-ip`).

## Install

Install `@dshplus/fake-ip` in the DSH Plugin Manager (or install the [`@dshplus/bundle`](https://github.com/ArakawaHenri/dshplus) bundle):

```text
@dshplus/fake-ip
```

## Why & How

TUN fake-IP DNS resolves domains into the reserved `198.18.0.0/15` range. DSH's built-in HTTP fetcher treats non-public IPs as SSRF risks and blocks them with `WEB_BLOCKED_URL`.

This plugin replaces the built-in `web-fetch-http` provider:
- **`fake-ip` mode (default)**: Allows `198.18.0.0/15` (or your custom `allowCidrs`) while keeping loopback, LAN, and other private ranges blocked.
- **`off` mode**: Skips the public-address check completely, useful if you intentionally want the agent to fetch from `localhost` or LAN services.

## Configuration

It works out of the box for standard IPv4 fake-IP setups. If you also use IPv6 fake-IP (such as `fc00::/18`) or want `off` mode, configure it in your profile's `cordis.patch.yml`:

```yaml
- id: dshplus-fake-ip
  config:
    mode: fake-ip
    allowCidrs:
      - 198.18.0.0/15
      - fc00::/18
```

| Option | Default | Description |
| --- | --- | --- |
| `mode` | `fake-ip` | `fake-ip` allows `allowCidrs`; `off` allows all private/loopback addresses |
| `allowCidrs` | `['198.18.0.0/15']` | IPv4 / IPv6 CIDRs allowed in `fake-ip` mode |
| `maxResponseBytes` | `5000000` | Max response size in bytes |
| `maxBodyChars` | `100000` | Max returned text length in characters |
| `timeoutMs` | `30000` | Request timeout in milliseconds |
| `maxRedirects` | `5` | Max same-origin redirects |
| `userAgent` | Built-in UA | Custom `User-Agent` header |

## License

MIT
