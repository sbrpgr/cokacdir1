# Dependency security and compatibility

Build from the committed Cargo.lock with `cargo build --locked`. The security
CI checks and tests the Rust application and audits/builds the website. The
prebuilt files in `dist/` and `dist_beta/` predate this update; they have not
been rebuilt or certified by this source change.

## SSH compatibility

The patched russh 0.62.7 build deliberately excludes the optional RustCrypto
RSA feature. RUSTSEC-2023-0071 has no patched release as of 2026-09-17:
https://rustsec.org/advisories/RUSTSEC-2023-0071.html

Use Ed25519/ECDSA SSH keys, or password authentication with a supported server
host key. RSA user keys and RSA-only servers may no longer connect. No user
keys or server settings are modified. Do not re-enable the vulnerable feature
merely to silence a compatibility error; review the upstream fix first.

Discord uses Serenity's supported native TLS backend to avoid its old rustls
dependency chain. Telegram and other HTTP clients retain their configured TLS
backends. Upgraded terminal/image APIs are covered by compilation and existing
unit tests; interactive remote-server and terminal-protocol testing remains a
release validation responsibility.

## Maintenance advisories

The lockfile scan also reports `paste` (through image codecs) and
`proc-macro-error2` (through Teloxide documentation macros) as unmaintained.
These are build-time maintenance advisories, not patched runtime CVEs. They
are recorded without hiding them with scanner exclusions. Follow upstream
replacements and re-audit the full lockfile on dependency updates.
