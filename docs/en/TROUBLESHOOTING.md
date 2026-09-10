# Troubleshooting

[English](TROUBLESHOOTING.md) | [Русский](../ru/TROUBLESHOOTING.md)

## Device detection

```bash
lsusb | grep -i '04f3:0c4b'
```

## fprintd status and journal

```bash
systemctl --no-pager --full status fprintd
journalctl -u fprintd -b --no-pager
```

## TOD module

```bash
ls -la /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/
readlink -f /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so
```

Expected target:

```text
/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
```

## RUNPATH

```bash
patchelf --print-rpath /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
```

Expected: `/opt/elan-fingerprint/lib`.

## Dependencies

```bash
ldd /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
```

There should be no `not found` entries. In particular, `libcrypto.so.1.1` should resolve to `/opt/elan-fingerprint/lib/libcrypto.so.1.1`.

The simultaneous presence of system `libcrypto.so.3` through transitive dependencies is not an error by itself.

## Check libraries actually loaded

While `fprintd` is running:

```bash
pid=$(pidof fprintd)
sudo grep -E 'elan-fingerprint|libcrypto\.so\.1\.1' "/proc/$pid/maps"
```

Expected paths should point into `/opt/elan-fingerprint`.

## Enrollment and verification

```bash
fprintd-list "$USER"
fprintd-verify
```

The working device is named `ELAN Fingerprint Sensor (press)`. If the stock/open-source path `ElanTech Fingerprint Sensor` is used instead, check the TOD symlink, udev rule, and journal.

## PAM recovery

If fingerprint authentication behaves incorrectly but `sudo` is still available:

```bash
sudo pam-auth-update
```

Disable **Fingerprint authentication**, then run:

```bash
sudo -k
sudo -v
```

If PAM backups were saved earlier, do not restore them mechanically after other system changes. Compare the backup with the current PAM files first.

## LED

A non-blinking LED does not mean the fingerprint sensor is not working. LED control is not part of the current package.
