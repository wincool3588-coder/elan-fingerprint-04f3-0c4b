# Security Policy

[Русская версия](SECURITY.ru.md)

## Supported versions

This project is small and currently supports only the latest published repository state and the latest documented package release unless a security notice explicitly says otherwise.

| Version | Supported |
| --- | --- |
| Latest release | Yes |
| Older releases | Best effort only |
| Unreleased or locally modified builds | No guarantee |

Security support here refers to project-authored packaging, scripts, documentation, and integration behavior. Proprietary ELAN components and other third-party binaries remain governed by their own vendors and terms.

## Reporting a vulnerability

Do **not** open a public issue for a vulnerability that could put users at risk.

Preferred path: use GitHub's private vulnerability reporting for this repository from the repository **Security** area when that option is available.

If private vulnerability reporting is not available, contact the maintainer through GitHub first without posting exploit details, credentials, personal data, or other sensitive information publicly. Coordinate a private disclosure path before sending sensitive technical details.

Please include, when relevant:

- affected release or commit
- Ubuntu and kernel versions
- device model and USB ID
- package version
- concise impact description
- reproduction steps or proof of concept
- whether the issue affects authentication, privilege boundaries, package installation, file permissions, library loading, or update/removal behavior
- suggested mitigation, if known

## What to expect

A valid report will be assessed for scope and reproducibility. The maintainer may ask for additional diagnostics. Public disclosure should wait until a fix or reasonable mitigation is available, unless immediate disclosure is necessary to protect users.

## Scope notes

This repository is an unofficial integration project and is not affiliated with Ubuntu, Lenovo, ELAN, or OpenSSL. Vulnerabilities that are solely in an upstream or proprietary third-party component may need to be reported to that vendor as well.

For licensing and redistribution concerns rather than security vulnerabilities, use a normal issue and the `legal/licensing` label instead of the security channel.
