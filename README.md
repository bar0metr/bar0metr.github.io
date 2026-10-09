# GoOS Trust Updates

This repository hosts the public, signed CA trust-store update channel for [GoOS](https://github.com/bar0metr/goos).

Stable production base URL:

```text
https://bar0metr.github.io/current
```

The `current/` directory is expected to contain:

```text
ca-certificates.pem
ca-certificates.json
ca-certificates.sig
```

GoOS verifies every package with the immutable Ed25519 update public key embedded in the system image. HTTPS protects transport; the package signature authorizes the update.

The production signing private key must never be committed to this repository.
