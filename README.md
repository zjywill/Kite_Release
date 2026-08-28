# Kite

**A network debugging proxy for macOS.** Capture, inspect, and rewrite HTTP and
HTTPS traffic on your Mac — the same job Charles Proxy and Proxyman do, built
natively for macOS 26 with SwiftUI and Apple's own networking stack.

This repository hosts signed release builds and the Sparkle update feed.

> **Beta.** Kite is under active development and has not yet been through a full
> manual acceptance pass. Expect rough edges. Do not rely on it for anything you
> cannot afford to have break.

## Download

Grab the latest `.zip` from [Releases](https://github.com/zjywill/Kite_Release/releases),
unzip it, and drag `Kite.app` into `/Applications`.

Builds are a universal binary (Apple Silicon and Intel), signed with a Developer
ID certificate and notarized by Apple, so Gatekeeper opens them without a
right-click dance. Once installed, Kite updates itself through Sparkle.

**Requires macOS 26 or later.** There is no compatibility path for older
releases — Kite is built against macOS 26 APIs only.

## What it does

### Capture

Every request that passes through Kite lands in a live capture list: method,
URL, status, timing, headers, and bodies. Requests are paired with their
responses as the bytes stream by, so the list stays correct through chunked
transfers, `HEAD` responses, `204`/`304`, `1xx` interim responses, and protocol
upgrades.

### HTTPS decryption

Kite can decrypt TLS by presenting a leaf certificate signed by its own root CA,
which you install and trust once.

**Decryption is opt-in per host and off by default.** There is no "decrypt
everything" switch, deliberately. Hosts that are not on your list are forwarded
byte-for-byte without being parsed at all. The root CA private key lives only in
your Keychain — it is never written to the configuration directory, which is the
kind of place that gets synced, backed up, and copied around.

Decryption parses **HTTP/1.1**. Connections negotiating other protocols are
tunnelled through untouched rather than mangled.

### Rewriting

- **Header rewrite** — add, replace, or drop request and response headers.
- **URL redirect** — rewrite the path in place, or answer with a `302`/`307`.
- **Mock** — return a canned response without the request ever reaching the
  server.
- **Body rewrite** — modify response bodies, with `Content-Length` recomputed.

### Proxying

Underneath the capture layer, Kite is a full rule-based proxy client. It reads
Clash-style YAML configuration, including remote subscriptions and rule
providers, and matches traffic first-rule-wins by domain, IP, GeoIP, and — on
macOS specifically — **process name**.

Outbound protocols: HTTP, SOCKS5, Shadowsocks, Trojan, VMess, VLESS/REALITY, and
Hysteria 2.

It can run as a plain system proxy, or in **enhanced mode** through a Network
Extension packet tunnel, which catches traffic from apps that ignore system proxy
settings.

## What it deliberately does not do

**No scripting.** No request scripts, no response scripts, no rule scripts, no
script-driven panels. Anything worth doing gets built as a real feature with a
real interface instead of a hook where you paste JavaScript. This is a
deliberate trade-off, not a missing feature — if you need a scriptable proxy,
Kite is the wrong tool.

## Verifying what you downloaded

```sh
codesign -dv --verbose=4 /Applications/Kite.app
xcrun stapler validate /Applications/Kite.app
spctl --assess --type execute --verbose=4 /Applications/Kite.app
```

You should see `Authority=Developer ID Application: Junyi Zhang (NGM7GX8DGB)`,
`flags=0x10000(runtime)`, and `source=Notarized Developer ID`.

## A word on trust

A tool that decrypts your HTTPS traffic is a tool that can read everything you
do online. Install its root CA only if you understand what that means, keep the
decryption host list as short as the job requires, and remove the certificate
when you are done debugging.

## License

AGPL-3.0.
