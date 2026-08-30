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
