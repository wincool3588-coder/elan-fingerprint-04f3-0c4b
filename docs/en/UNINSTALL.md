# Safe complete removal

[English](UNINSTALL.md) | [Русский](../ru/UNINSTALL.md)

This procedure is intended for a system where fingerprint authentication is already enabled in PAM.

## 1. Disable fingerprint authentication in PAM first

Make sure you know a working password, then run:

```bash
sudo pam-auth-update
```

Disable **Fingerprint authentication**. Do not disable normal Unix password authentication.

## 2. Prove that password authentication works

```bash
sudo -k
sudo -v
```

Authentication must succeed with the password. Preferably also lock the screen and verify password login.

**Do not proceed with driver removal until this test passes.**

## 3. Optionally remove enrolled fingerprints

If you want to remove biometric data completely while the driver still works:

```bash
fprintd-delete "$USER"
fprintd-list "$USER"
```

If you only want to disable fingerprint authentication, removing enrollment is optional.

## 4. Purge the package

For version `1.0.2` and later:

```bash
sudo apt purge elan-fingerprint-04f3-0c4b
```

Alternative without purge:

```bash
sudo dpkg -r elan-fingerprint-04f3-0c4b
```

`purge` is preferred for complete removal because the udev rule is a conffile.

If you are still using the historical `1.0.0+local1` package, its package name is `elan-fingerprint-04f3-0c4b-local`.

## 5. Verify the result

```bash
dpkg-query -W elan-fingerprint-04f3-0c4b
ls -la /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/
test ! -e /etc/udev/rules.d/60-libfprint-2-tod1-elan.rules && echo 'udev rule removed'
test ! -e /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so && echo 'driver removed'
test ! -e /opt/elan-fingerprint/lib/libcrypto.so.1.1 && echo 'private libcrypto removed'
```

## 6. Local backup files

Before packaging, local files such as `working-config.txt`, `ROLLBACK.txt`, `PAM-ROLLBACK.txt`, and `pam-before/` may have been created under `/opt/elan-fingerprint/docs`. They are not necessarily package-owned and may remain after purge.

Inspect the tree:

```bash
find /opt/elan-fingerprint -maxdepth 3 -ls
```

Only if nothing important remains:

```bash
sudo rm -rf /opt/elan-fingerprint
```

Never run that final command blindly.

## Correct order

```text
working password
 -> disable Fingerprint authentication in pam-auth-update
 -> verify sudo with password
 -> verify lock screen with password
 -> optionally delete enrollment
 -> apt purge elan-fingerprint-04f3-0c4b
 -> inspect leftovers
 -> separately decide what to do with local backup files
```
