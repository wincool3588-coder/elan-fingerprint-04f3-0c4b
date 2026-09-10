# Устройство пакета

[English](../en/PACKAGE.md) | [Русский](PACKAGE.md)

Package: `elan-fingerprint-04f3-0c4b-local`

Version: `1.0.0+local1`

Architecture: `amd64`

## Dependencies

```text
libfprint-2-tod1
fprintd
```

PAM configuration намеренно не принадлежит package и управляется отдельно через `pam-auth-update`.

## Устанавливаемые файлы

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

Proprietary driver patched с абсолютным RUNPATH:

```text
/opt/elan-fingerprint/lib
```

Это изолирует legacy `libcrypto.so.1.1` от остальной системы.

## Maintainer scripts

`postinst` и `postrm` перезагружают udev rules, trigger USB subsystem и используют `systemctl try-restart fprintd.service`. Эти операции best-effort и не изменяют PAM.

## Проверка ownership

```bash
dpkg-query -S \
  /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so \
  /opt/elan-fingerprint/lib/libcrypto.so.1.1 \
  /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so \
  /etc/udev/rules.d/60-libfprint-2-tod1-elan.rules
```

Все четыре объекта должны принадлежать `elan-fingerprint-04f3-0c4b-local`.
