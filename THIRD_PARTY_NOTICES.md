# Third-party notices

[English](THIRD_PARTY_NOTICES.md) | [Русский](THIRD_PARTY_NOTICES.ru.md)

This repository documents and packages third-party components required to operate an ELAN `04f3:0c4b` fingerprint reader with Ubuntu's libfprint TOD stack.

## Scope of the repository license

The Apache License 2.0 in [`LICENSE`](LICENSE) applies only to material authored specifically for this repository, such as original documentation, packaging metadata, maintainer scripts, and other original project code.

It does **not** relicense third-party software or binary components. Each third-party component remains subject to its own copyright and license terms. Inclusion, reference, or packaging of a third-party component must not be interpreted as a grant of rights beyond those provided by that component's copyright holder or applicable license.

## ELAN TOD binary

`libfprint-2-tod1-elan.so` is a proprietary/redistributed binary associated with ELAN/Lenovo fingerprint support. It is **not authored by this repository**, is not covered by this repository's Apache-2.0 license, and is not claimed to be open-source.

The working binary used for package `1.0.0+local1` has SHA256:

```text
be47d4e63bdb541397d1a0d853115d034c36179580aade16e3b52bdcd1dbc3f0
```

Source/reference used during recovery work: TonyHoyle's `libfprint-2-tod1-elan` repository and the `Abishek-Pechiappan/libfprint-elan-04f3-0c4b-tod` device-specific guide.

**Redistribution status:** unresolved. Redistribution rights for the proprietary blob are not established by the documentation or Apache-2.0 license in this repository. Anyone redistributing the binary must independently verify the applicable Lenovo/ELAN licensing terms. Until that status is resolved, the presence of a copy in this repository or in a generated package should not be interpreted as an assertion that redistribution is authorized.

## OpenSSL 1.1 compatibility library

The package contains only `libcrypto.so.1.1` extracted from Ubuntu Focal's official `libssl1.1` package, rather than installing the obsolete package globally. The binary used here has SHA256:

```text
bf99926de2ce739d3fdccc4a551d97d64aac6e968589aaad9dc4ac8d143b519a
```

It is isolated under `/opt/elan-fingerprint/lib` and selected by the ELAN driver's private RUNPATH.

OpenSSL is third-party software and is **not** relicensed under this repository's Apache-2.0 license. The exact license and corresponding source/redistribution obligations must be determined from the Ubuntu Focal `libssl1.1` package metadata and its corresponding source package before treating this repository's binary package as generally redistributable.

## Repository documentation and packaging

Original documentation, packaging metadata, maintainer scripts, and other material authored specifically for this repository are licensed under Apache License 2.0 unless a file explicitly states otherwise.

Those terms do not change, replace, or supersede the licenses or redistribution restrictions of bundled or referenced third-party components.

## Legal status tracking

The remaining redistribution questions are tracked in GitHub Issue #1. Until they are resolved, release artifacts containing the ELAN TOD binary should be treated as having unresolved third-party redistribution status.
