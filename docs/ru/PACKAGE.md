# Устройство пакета

[English](../en/PACKAGE.md) | [Русский](PACKAGE.md)

Пакет: `elan-fingerprint-04f3-0c4b-local`

Версия: `1.0.0+local1`

Архитектура: `amd64`

## Зависимости

```text
libfprint-2-tod1
fprintd
```

Конфигурация PAM намеренно не входит в состав пакета и управляется отдельно с помощью `pam-auth-update`.

## Устанавливаемые файлы

```text
/etc/udev/rules.d/60-libfprint-2-tod1-elan.rules
/opt/elan-fingerprint/docs/README
/opt/elan-fingerprint/docs/SHA256SUMS
/opt/elan-fingerprint/docs/SOURCES
/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
/opt/elan-fingerprint/lib/libcrypto.so.1.1
/usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so
/usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/LICENSE
/usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/SOURCE
```

TOD-объект в `/usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/` — символическая ссылка на `/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so`.

Файлы OpenSSL в `/usr/share/doc/.../third-party/openssl/` сохраняют полный текст лицензии OpenSSL 1.1.1f и документируют точное происхождение приватной копии `libcrypto.so.1.1` из Ubuntu binary/source package.

## Контрольные суммы бинарных файлов

```text
be47d4e63bdb541397d1a0d853115d034c36179580aade16e3b52bdcd1dbc3f0  /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
bf99926de2ce739d3fdccc4a551d97d64aac6e968589aaad9dc4ac8d143b519a  /opt/elan-fingerprint/lib/libcrypto.so.1.1
```

## Provenance и лицензия OpenSSL

Bundled `libcrypto.so.1.1` извлечена из:

```text
libssl1.1_1.1.1f-1ubuntu2.24_amd64.deb
SHA256: 7cf39d70a639017d1dd7c8d36daa2258063608688e449fddf40ffdd46f992a78
```

Соответствующий Ubuntu source package:

```text
openssl 1.1.1f-1ubuntu2.24
```

OpenSSL 1.1.1f распространяется по OpenSSL License и Original SSLeay License; применяются обе лицензии. Package source tree содержит полный upstream license text и provenance record в `/usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/`.

См. [`../../THIRD_PARTY_NOTICES.ru.md`](../../THIRD_PARTY_NOTICES.ru.md) для repository-level сведений о лицензировании сторонних компонентов.

## Контрольная сумма пакета

Опубликованный package `1.0.0+local1` сейчас имеет:

```text
9893258dfeb04259312fccb465ac372440e2e972787966794e63b87c9c520b44  elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb
```

Эта контрольная сумма относится к существующему опубликованному artifact. Package, пересобранный после добавления OpenSSL notice files, неизбежно получит другую контрольную сумму; перед публикацией её нужно зафиксировать отдельно.

## RUNPATH

Проприетарный драйвер настроен с абсолютным RUNPATH:

```text
/opt/elan-fingerprint/lib
```

Это изолирует устаревшую `libcrypto.so.1.1` от остальной системы.

## Скрипты сопровождения

`postinst` и `postrm` перезагружают правила udev, инициируют обработку USB-устройств и выполняют `systemctl try-restart fprintd.service`. Эти операции выполняются по принципу best effort и не изменяют конфигурацию PAM.

## Проверка принадлежности файлов пакету

```bash
dpkg-query -S \
  /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so \
  /opt/elan-fingerprint/lib/libcrypto.so.1.1 \
  /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so \
  /usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/LICENSE \
  /usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/SOURCE \
  /etc/udev/rules.d/60-libfprint-2-tod1-elan.rules
```

Все объекты должны принадлежать пакету `elan-fingerprint-04f3-0c4b-local` после пересборки из текущего package tree.
