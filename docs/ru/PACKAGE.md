# Устройство пакета

[English](../en/PACKAGE.md) | [Русский](PACKAGE.md)

Пакет: `elan-fingerprint-04f3-0c4b`

Версия: `1.0.2`

Архитектура: `amd64`

## Зависимости

```text
libfprint-2-tod1
fprintd
```

Конфигурация PAM намеренно не входит в состав пакета и управляется отдельно с помощью `pam-auth-update`.

## Переход со старого имени пакета

В версии `1.0.2` удалены суффикс `-local` из имени пакета и `+local1` из версии Debian. В метаданных пакета указано:

```text
Replaces: elan-fingerprint-04f3-0c4b-local (<< 1.0.2)
Conflicts: elan-fingerprint-04f3-0c4b-local (<< 1.0.2)
```

Это позволяет новому пакету забрать файлы, принадлежавшие ранее опубликованному пакету `elan-fingerprint-04f3-0c4b-local 1.0.0+local1`, и не оставлять в системе два пакета, владеющих одинаковыми путями.

## Устанавливаемые файлы

```text
/etc/udev/rules.d/60-libfprint-2-tod1-elan.rules
/opt/elan-fingerprint/docs/README
/opt/elan-fingerprint/docs/SHA256SUMS
/opt/elan-fingerprint/docs/SOURCES
/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
/opt/elan-fingerprint/lib/libcrypto.so.1.1
/usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so
/usr/share/doc/elan-fingerprint-04f3-0c4b/third-party/openssl/LICENSE
/usr/share/doc/elan-fingerprint-04f3-0c4b/third-party/openssl/SOURCE
```

Файл в `/usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/` является символической ссылкой на `/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so`.

Файлы OpenSSL в `/usr/share/doc/.../third-party/openssl/` содержат полный текст лицензии OpenSSL 1.1.1f и сведения о точном происхождении включённой в пакет копии `libcrypto.so.1.1` из бинарного пакета и соответствующего пакета с исходным кодом Ubuntu.

## Контрольные суммы бинарных файлов

```text
be47d4e63bdb541397d1a0d853115d034c36179580aade16e3b52bdcd1dbc3f0  /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
bf99926de2ce739d3fdccc4a551d97d64aac6e968589aaad9dc4ac8d143b519a  /opt/elan-fingerprint/lib/libcrypto.so.1.1
```

Эти контрольные суммы бинарных компонентов не изменились по сравнению с ранее опубликованным пакетом.

## Происхождение и лицензия OpenSSL

Включённая `libcrypto.so.1.1` извлечена из пакета:

```text
libssl1.1_1.1.1f-1ubuntu2.24_amd64.deb
SHA256: 7cf39d70a639017d1dd7c8d36daa2258063608688e449fddf40ffdd46f992a78
```

Соответствующий пакет с исходным кодом Ubuntu:

```text
openssl 1.1.1f-1ubuntu2.24
```

OpenSSL 1.1.1f распространяется одновременно на условиях OpenSSL License и Original SSLeay License. Пакет содержит полный исходный текст лицензии и сведения о происхождении библиотеки в `/usr/share/doc/elan-fingerprint-04f3-0c4b/third-party/openssl/`.

Сведения о лицензировании сторонних компонентов на уровне репозитория приведены в [`../../THIRD_PARTY_NOTICES.ru.md`](../../THIRD_PARTY_NOTICES.ru.md).

## Контрольная сумма пакета

Пакет `1.0.2` имеет следующую контрольную сумму:

```text
7550ff3373fa5b1c3ef9f62e51fc7a6ab63c18bd59ec2c8fecbff3b69ee64fe9  elan-fingerprint-04f3-0c4b_1.0.2_amd64.deb
```

Для исторической справки: ранее был опубликован пакет `elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb` с SHA256 `9893258dfeb04259312fccb465ac372440e2e972787966794e63b87c9c520b44`.

## RUNPATH

Проприетарный драйвер настроен с абсолютным RUNPATH:

```text
/opt/elan-fingerprint/lib
```

Это изолирует устаревшую `libcrypto.so.1.1` от остальной системы.

## Сценарии сопровождения

`postinst` и `postrm` перезагружают правила udev, инициируют обработку USB-устройств и выполняют `systemctl try-restart fprintd.service`. Эти операции выполняются по возможности и не изменяют конфигурацию PAM.

## Проверка принадлежности файлов пакету

```bash
dpkg-query -S \
  /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so \
  /opt/elan-fingerprint/lib/libcrypto.so.1.1 \
  /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so \
  /usr/share/doc/elan-fingerprint-04f3-0c4b/third-party/openssl/LICENSE \
  /usr/share/doc/elan-fingerprint-04f3-0c4b/third-party/openssl/SOURCE \
  /etc/udev/rules.d/60-libfprint-2-tod1-elan.rules
```

После установки версии `1.0.2` все перечисленные объекты должны принадлежать пакету `elan-fingerprint-04f3-0c4b`.
