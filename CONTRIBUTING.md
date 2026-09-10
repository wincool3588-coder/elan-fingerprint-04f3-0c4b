# Contributing

[Русская версия](CONTRIBUTING.ru.md)

Thank you for helping improve ELAN `04f3:0c4b` fingerprint support.

## Before opening an issue

- Search existing issues first.
- Use the dedicated bug, compatibility, or feature request form when available.
- Do not publish security vulnerabilities in a public issue; follow [SECURITY.md](SECURITY.md).
- For licensing or redistribution questions involving proprietary ELAN components or `libcrypto.so.1.1`, use the `legal/licensing` label and refer to [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

## Bug reports

Please include enough information to reproduce and diagnose the problem:

- laptop or device model
- Ubuntu version
- kernel version (`uname -r`)
- fingerprint reader USB ID (`lsusb`)
- versions of `fprintd`, `libfprint-2-2`, `libfprint-2-tod1`, and `libpam-fprintd`
- installed version of `elan-fingerprint-04f3-0c4b`
- exact steps that reproduce the issue
- expected and actual behavior
- relevant command output or journal excerpts, with personal information removed

Useful checks are documented in [docs/en/TROUBLESHOOTING.md](docs/en/TROUBLESHOOTING.md).

## Compatibility reports

Compatibility results for other laptops, firmware revisions, kernels, or Ubuntu releases are welcome. Please use the compatibility report form and state clearly which of these were tested:

- enrollment
- `fprintd-verify`
- `sudo`
- GNOME lock screen
- graphical login
- password fallback

A failed compatibility result is useful too when it includes enough diagnostic information.

## Pull requests

1. Create a focused branch from the current `main` branch.
2. Keep each pull request limited to one logical change.
3. Update English canonical documentation first when behavior or policy changes, then update the Russian counterpart where one exists.
4. Preserve commands, paths, package names, hashes, USB IDs, error messages, and other technical literals when translating documentation.
5. Explain how the change was tested.
6. Link the related issue when applicable.

The repository uses pull requests for changes to `main` and squash merging to keep history linear.

## Third-party binaries

Do not add, replace, repackage, or redistribute proprietary ELAN binaries, OpenSSL compatibility libraries, firmware, or other third-party binary payloads in a pull request unless their source, license, redistribution terms, and checksums are explicitly documented and reviewed.

The Apache-2.0 license for repository-authored material does not relicense third-party components. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

## Style

- Keep documentation concise and reproducible.
- Prefer commands that can be copied exactly.
- Avoid destructive system-wide workarounds when a package-local solution is available.
- Do not recommend global `LD_LIBRARY_PATH`, global `ld.so.conf` changes, or unsafe `libcrypto` symlinks for this package.
