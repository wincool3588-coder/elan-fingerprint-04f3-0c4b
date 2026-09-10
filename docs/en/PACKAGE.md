# Package layout

[English](PACKAGE.md) | [Русский](../ru/PACKAGE.md)

Package: `elan-fingerprint-04f3-0c4b-local`

Version: `1.0.0+local1`

Architecture: `amd64`

## Dependencies

```text
libfprint-2-tod1
fprintd
```

PAM configuration is deliberately outside package ownership and is managed separately through `pam-auth-update`.

## Installed files

```text
/etc/udev/rules.d/60-libfprint-2-tod1-elan.rules
/opt/elan-fingerprint/docs/README
/opt/elan-fingerprint/docs/SHA256SUMS
/opt/elan-fingerprint/docs/SOURCES
/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
/opt/elan-fingerprint/lib/libcrypto.so.1.1
/usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so
/usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/LICENSE
/usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/SOURCE
```

The TOD item under `/usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/` is a symlink to `/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so`.

The OpenSSL files under `/usr/share/doc/.../third-party/openssl/` preserve the complete OpenSSL 1.1.1f license text and document the exact Ubuntu binary/source provenance of the private `libcrypto.so.1.1` copy.

## Binary checksums

```text
be47d4e63bdb541397d1a0d853115d034c36179580aade16e3b52bdcd1dbc3f0  /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
bf99926de2ce739d3fdccc4a551d97d64aac6e968589aaad9dc4ac8d143b519a  /opt/elan-fingerprint/lib/libcrypto.so.1.1
```

## OpenSSL provenance and license

The bundled `libcrypto.so.1.1` was extracted from:

```text
libssl1.1_1.1.1f-1ubuntu2.24_amd64.deb
SHA256: 7cf39d70a639017d1dd7c8d36daa2258063608688e449fddf40ffdd46f992a78
```

Its corresponding Ubuntu source package is:

```text
openssl 1.1.1f-1ubuntu2.24
```

OpenSSL 1.1.1f is distributed under the OpenSSL License and Original SSLeay License; both apply. The package source tree ships the complete upstream license text and provenance record under `/usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/`.

See [`../../THIRD_PARTY_NOTICES.md`](../../THIRD_PARTY_NOTICES.md) for the repository-level third-party licensing record.

## Package checksum

The published `1.0.0+local1` package currently has:

```text
9893258dfeb04259312fccb465ac372440e2e972787966794e63b87c9c520b44  elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb
```

That checksum describes the existing published artifact. A package rebuilt after adding the OpenSSL notice files will necessarily have a different package checksum and must be recorded separately before publication.

## RUNPATH

The proprietary driver is patched with an absolute RUNPATH:

```text
/opt/elan-fingerprint/lib
```

This isolates legacy `libcrypto.so.1.1` from the rest of the system.

## Maintainer scripts

`postinst` and `postrm` reload udev rules, trigger the USB subsystem, and use `systemctl try-restart fprintd.service`. These operations are best-effort and do not modify PAM.

## Ownership check

```bash
dpkg-query -S \
  /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so \
  /opt/elan-fingerprint/lib/libcrypto.so.1.1 \
  /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so \
  /usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/LICENSE \
  /usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/SOURCE \
  /etc/udev/rules.d/60-libfprint-2-tod1-elan.rules
```

All objects should belong to `elan-fingerprint-04f3-0c4b-local` in a package rebuilt from the current package tree.
