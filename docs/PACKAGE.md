# Package layout

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
```

Последний объект — symlink на `/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so`.

## Binary checksums

```text
be47d4e63bdb541397d1a0d853115d034c36179580aade16e3b52bdcd1dbc3f0  /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
bf99926de2ce739d3fdccc4a551d97d64aac6e968589aaad9dc4ac8d143b519a  /opt/elan-fingerprint/lib/libcrypto.so.1.1
```

## Package checksum

```text
9893258dfeb04259312fccb465ac372440e2e972787966794e63b87c9c520b44  elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb
```

## RUNPATH

Proprietary driver is patched with an absolute RUNPATH:

```text
/opt/elan-fingerprint/lib
```

This isolates legacy `libcrypto.so.1.1` from the rest of the system.

## Maintainer scripts

`postinst` and `postrm` reload udev rules, trigger the USB subsystem and use `systemctl try-restart fprintd.service`. These operations are best-effort and do not modify PAM.

## Ownership check

```bash
dpkg-query -S \
  /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so \
  /opt/elan-fingerprint/lib/libcrypto.so.1.1 \
  /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so \
  /etc/udev/rules.d/60-libfprint-2-tod1-elan.rules
```

Все четыре объекта должны принадлежать `elan-fingerprint-04f3-0c4b-local`.
