# ELAN 04f3:0c4b fingerprint support for Ubuntu 26.04 LTS

[English](README.md) | [Русский](README.ru.md)

Local Debian package and documentation for the **ELAN 04f3:0c4b** USB fingerprint reader, tested on a **Lenovo ThinkBook 14 G2 ARE** running **Ubuntu 26.04 LTS**.

> [!IMPORTANT]
> This is not an official Ubuntu, Lenovo, or ELAN package. The `.deb` contains a third-party proprietary ELAN TOD binary and a private compatibility copy of `libcrypto.so.1.1`. See [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md).

## Tested configuration

- Hardware: Lenovo ThinkBook 14 G2 ARE
- USB ID: `04f3:0c4b Elan Microelectronics Corp. ELAN:Fingerprint`
- Ubuntu: 26.04 LTS
- Kernel: `7.0.0-31-generic`
- `fprintd`: `1.94.5-4`
- `libfprint-2-2`: `1:1.95.1+tod1-0ubuntu2`
- `libfprint-2-tod1`: `1:1.95.1+tod1-0ubuntu2`
- `libpam-fprintd`: `1.94.5-4`
- Local package: `elan-fingerprint-04f3-0c4b-local 1.0.0+local1`

Enrollment, `fprintd-verify`, `sudo`, GNOME lock screen, graphical login, and password fallback were tested successfully.

## Architecture

```text
ELAN USB 04f3:0c4b
  -> udev rule
  -> libfprint TOD
  -> /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
  -> RUNPATH=/opt/elan-fingerprint/lib
  -> private libcrypto.so.1.1
  -> fprintd -> pam_fprintd -> sudo / lock screen / login
```

The legacy OpenSSL library is **not installed globally**. This project does not create `libcrypto.so.1.1 -> libcrypto.so.3`, modify `ld.so.conf`, run global `ldconfig` changes, or set a global `LD_LIBRARY_PATH`.

## Installation

Check that the device is present:

```bash
lsusb | grep -i '04f3:0c4b'
```

Check the downloaded `.deb` SHA256:

```bash
sha256sum elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb
```

Expected:

```text
9893258dfeb04259312fccb465ac372440e2e972787966794e63b87c9c520b44
```

Install the package:

```bash
sudo dpkg -i ./elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb
```

The package installs the TOD driver, private `libcrypto.so.1.1`, TOD symlink, and udev rule. **It does not modify PAM configuration.**

## Verification

```bash
patchelf --print-rpath /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
ldd /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so | grep -E 'crypto|not found'
sha256sum -c /opt/elan-fingerprint/docs/SHA256SUMS
fprintd-list "$USER"
fprintd-verify
```

A working sensor is detected as `ELAN Fingerprint Sensor (press)`, and verification ends with `Verify result: verify-match (done)`.

If no enrollment exists:

```bash
fprintd-enroll -f right-index-finger "$USER"
```

## PAM

Before enabling fingerprint authentication, make sure the normal password is known and works:

```bash
sudo pam-auth-update
```

Enable **Fingerprint authentication** while keeping the existing password methods enabled. First test fingerprint authentication with `sudo -k; sudo -v`. Then perform a separate test where fingerprint authentication times out and confirm that password fallback still works. Only after that should you test the lock screen and logout/login flow.

## Safe complete removal

**Do not remove the driver first while PAM still expects fingerprint authentication.**

1. Make sure you know a working password.
2. Run `sudo pam-auth-update`.
3. Disable **Fingerprint authentication**.
4. Run `sudo -k && sudo -v` and confirm password authentication works.
5. Preferably verify the lock screen with a password as well.
6. If you are removing biometrics completely, while the driver still works you may run `fprintd-delete "$USER"`.
7. Purge the package:

```bash
sudo apt purge elan-fingerprint-04f3-0c4b-local
```

8. Check package-owned files. Local backup files under `/opt/elan-fingerprint/docs` may remain because the package does not own them. Do not remove the entire `/opt/elan-fingerprint` tree blindly.

Full procedure: [`docs/en/UNINSTALL.md`](docs/en/UNINSTALL.md).

## LED

The TOD driver provides fingerprint scanning, but the power-button LED may not blink as it does in Windows. LED support is intentionally not part of this package and is tracked separately.

## Do not

- install legacy `libssl1.1` globally just for this driver;
- create `libcrypto.so.1.1 -> libcrypto.so.3`;
- add the private OpenSSL library to `/etc/ld.so.conf*`;
- set a global `LD_LIBRARY_PATH`;
- replace the Ubuntu suite for an old PPA;
- remove the system libfprint stack;
- remove the driver before verifying password authentication.

## Documentation

- [`docs/en/PACKAGE.md`](docs/en/PACKAGE.md) — package layout and validation.
- [`docs/en/UNINSTALL.md`](docs/en/UNINSTALL.md) — safe complete removal.
- [`docs/en/TROUBLESHOOTING.md`](docs/en/TROUBLESHOOTING.md) — diagnostics and recovery.
- [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) — origin and licensing status of third-party components.

Russian versions are available under [`README.ru.md`](README.ru.md), [`docs/ru/`](docs/ru/), and [`THIRD_PARTY_NOTICES.ru.md`](THIRD_PARTY_NOTICES.ru.md).

## License

Material authored specifically for this repository — including original documentation, packaging metadata, maintainer scripts, and other original project code — is provided under the **Apache License 2.0**. See [`LICENSE`](LICENSE).

This does **not** mean Apache-2.0 applies to third-party binary components. In particular, the proprietary ELAN TOD driver and `libcrypto.so.1.1` remain subject to their own applicable licensing terms and are described separately in [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md).

For `libcrypto.so.1.1`, [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) records the exact Ubuntu binary package, verified checksums, corresponding Ubuntu source package, and the applicable OpenSSL 1.1.1f licensing terms. OpenSSL 1.1.1f is covered by both the **OpenSSL License and Original SSLeay License**; it is not relicensed under Apache-2.0. The complete upstream license text and provenance record are also included in the package tree.

Until redistribution rights for the ELAN TOD binary are confirmed, the presence of a copy of the binary or package in this repository must not be interpreted as a grant of rights to that component by the repository author.
