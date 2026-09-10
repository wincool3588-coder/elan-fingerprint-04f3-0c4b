# Third-party notices

[English](THIRD_PARTY_NOTICES.md) | [Русский](THIRD_PARTY_NOTICES.ru.md)

This repository documents and packages third-party components required to operate an ELAN `04f3:0c4b` fingerprint reader with Ubuntu's libfprint TOD stack.

## Scope of the repository license

The Apache License 2.0 in [`LICENSE`](LICENSE) applies only to material authored specifically for this repository, such as original documentation, packaging metadata, maintainer scripts, and other original project code.

It does **not** relicense third-party software or binary components. Each third-party component remains subject to its own copyright and license terms. Inclusion, reference, or packaging of a third-party component must not be interpreted as a grant of rights beyond those provided by that component's copyright holder or applicable license.

## ELAN TOD binary

`libfprint-2-tod1-elan.so` is a proprietary/redistributed binary associated with ELAN/Lenovo fingerprint support. It is **not authored by this repository**, is not covered by this repository's Apache-2.0 license, and is not claimed to be open-source.

The working binary used for package `1.0.2` has SHA256:

```text
be47d4e63bdb541397d1a0d853115d034c36179580aade16e3b52bdcd1dbc3f0
```

This is the same ELAN TOD binary payload used in the previously published `1.0.0+local1` package.

Source/reference used during recovery work: TonyHoyle's `libfprint-2-tod1-elan` repository and the `Abishek-Pechiappan/libfprint-elan-04f3-0c4b-tod` device-specific guide.

**Redistribution status:** unresolved. Redistribution rights for the proprietary blob are not established by the documentation or Apache-2.0 license in this repository. Anyone redistributing the binary must independently verify the applicable Lenovo/ELAN licensing terms. Until that status is resolved, the presence of a copy in this repository or in a generated package should not be interpreted as an assertion that redistribution is authorized.

## OpenSSL 1.1 compatibility library

The package contains only `libcrypto.so.1.1`, isolated under `/opt/elan-fingerprint/lib` and selected by the ELAN driver's private RUNPATH. It was extracted from the official Ubuntu 20.04 LTS (Focal) amd64 package:

```text
libssl1.1_1.1.1f-1ubuntu2.24_amd64.deb
SHA256: 7cf39d70a639017d1dd7c8d36daa2258063608688e449fddf40ffdd46f992a78
```

The bundled library has SHA256:

```text
bf99926de2ce739d3fdccc4a551d97d64aac6e968589aaad9dc4ac8d143b519a
```

The SHA256 of the bundled library was independently verified after extracting it from the named Ubuntu binary package.

The corresponding Ubuntu source package is:

```text
openssl 1.1.1f-1ubuntu2.24
```

Ubuntu publishes the corresponding source materials as:

```text
openssl_1.1.1f.orig.tar.gz
SHA256: 186c6bfe6ecfba7a5b48c47f8a1673d0f3b0e5ba2e25602dd23b629975da3f35

openssl_1.1.1f-1ubuntu2.24.debian.tar.xz
SHA256: 66b1a31642710d386b6896e2e7bea0bd3138d94277c894f871ccaa52bad07c04

openssl_1.1.1f-1ubuntu2.24.dsc
SHA256: f9b93b532511ee24b3e0160c0c7549d3e3123c9e2d9c5c6da0e6de2f582eccd3
```

OpenSSL 1.1.1f is distributed under the **OpenSSL License and Original SSLeay License; both apply**. Those licenses permit redistribution in binary form provided their notice, attribution, and disclaimer conditions are preserved. OpenSSL remains third-party software and is **not** relicensed under this repository's Apache-2.0 license.

The package ships the complete upstream OpenSSL 1.1.1f license text and provenance information at:

```text
/usr/share/doc/elan-fingerprint-04f3-0c4b/third-party/openssl/LICENSE
/usr/share/doc/elan-fingerprint-04f3-0c4b/third-party/openssl/SOURCE
```

The required acknowledgements are retained with those materials, including attribution to the OpenSSL Project and Eric Young.

Official source-package information: <https://launchpad.net/ubuntu/+source/openssl/1.1.1f-1ubuntu2.24>

Upstream license source: <https://github.com/openssl/openssl/blob/OpenSSL_1_1_1f/LICENSE>

## Repository documentation and packaging

Original documentation, packaging metadata, maintainer scripts, and other material authored specifically for this repository are licensed under Apache License 2.0 unless a file explicitly states otherwise.

Those terms do not change, replace, or supersede the licenses or redistribution restrictions of bundled or referenced third-party components.

## Legal status tracking

The remaining redistribution questions are tracked in GitHub Issue #1. The OpenSSL 1.1 compatibility component now has documented provenance, source-package mapping, and bundled license notices. The proprietary ELAN TOD binary remains unresolved, so release artifacts containing it should still be treated as having unresolved third-party redistribution status.
