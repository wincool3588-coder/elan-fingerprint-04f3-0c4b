# Third-party notices

This repository documents and packages third-party components required to operate an ELAN `04f3:0c4b` fingerprint reader with Ubuntu's libfprint TOD stack.

## ELAN TOD binary

`libfprint-2-tod1-elan.so` is a proprietary/redistributed binary associated with ELAN/Lenovo fingerprint support. It is **not authored by this repository** and is not claimed to be open-source.

The working binary used for package `1.0.0+local1` has SHA256:

```text
be47d4e63bdb541397d1a0d853115d034c36179580aade16e3b52bdcd1dbc3f0
```

Source/reference used during recovery work: TonyHoyle's `libfprint-2-tod1-elan` repository and the `Abishek-Pechiappan/libfprint-elan-04f3-0c4b-tod` device-specific guide.

Redistribution rights for the proprietary blob are not granted by the documentation in this repository. Anyone redistributing the binary should independently verify the applicable Lenovo/ELAN licensing terms.

## OpenSSL 1.1 compatibility library

The package contains only `libcrypto.so.1.1` extracted from Ubuntu Focal's official `libssl1.1` package, rather than installing the obsolete package globally. The binary used here has SHA256:

```text
bf99926de2ce739d3fdccc4a551d97d64aac6e968589aaad9dc4ac8d143b519a
```

It is isolated under `/opt/elan-fingerprint/lib` and selected by the ELAN driver's private RUNPATH.

OpenSSL is third-party software subject to its own license terms. Ubuntu package metadata and source packages should be consulted for the exact corresponding licensing/source information.

## Repository documentation

Documentation and packaging metadata written specifically for this repository do not change or supersede the licenses of any bundled third-party binary.
