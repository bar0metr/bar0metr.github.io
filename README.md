# GoOS Trust Updates

This repository hosts the public, signed CA trust-store update channel for [GoOS](https://github.com/bar0metr/goos).

Stable production base URL:

```text
https://bar0metr.github.io/current
```

The `current/` directory publishes the complete package:

```text
ca-certificates.pem
ca-certificates.json
ca-certificates.sig
```

GoOS verifies every package with the immutable Ed25519 update public key embedded in the system image. HTTPS protects transport; the package signature authorizes the update.

The production signing private key must never be committed to this repository.

## Current publication

- Trust sequence: `2026101001` (independent of the GoOS release).
- Mozilla source date: `2026-09-25T03:12:01Z`.
- Roots: 121; canonical PEM: 181603 bytes.
- Bundle SHA-256: `b5e44e6cf3ec2cda6131fec4e60a358ed022af5d5a8584da589b1851a56d0bb5`.
- Signature: 64 raw Ed25519 bytes.

This first publication uses the existing reviewed bootstrap roots. The sequence
marks a new signed package publication, not a new upstream source date.
See [MPL 2.0](LICENSE-MPL-2.0) and the
[dated upstream source](https://curl.se/ca/cacert-2026-09-25.pem).

## Publishing a replacement

From a GoOS checkout, prepare a package in an external staging directory using
`make trust-update` with an increasing sequence, the recorded upstream date,
local CA source and an explicitly supplied external signing seed. Review the
root-change report. Never upload the seed or test fixtures.

Verify staged and downloaded packages with the immutable production public key:

```sh
go run ./tools/truststore -verify -public-key security/trust-update-key.pub \
  /external/package/ca-certificates.pem \
  /external/package/ca-certificates.json \
  /external/package/ca-certificates.sig
```

Replace PEM, JSON and SIG in one commit to this repository, preserving the
complete package and updating the human-readable index. Publish the commit,
then download and verify all three deployed files. During deployment or cache
skew, a mixed generation fails signature/digest verification; it never replaces
installed trust. Existing GoOS installations retain their current valid roots
until a complete newer package passes verification.
