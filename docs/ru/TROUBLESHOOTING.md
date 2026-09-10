# Диагностика

[English](../en/TROUBLESHOOTING.md) | [Русский](TROUBLESHOOTING.md)

## Определение устройства

```bash
lsusb | grep -i '04f3:0c4b'
```

## Статус fprintd и журнал

```bash
systemctl --no-pager --full status fprintd
journalctl -u fprintd -b --no-pager
```

## Модуль TOD

```bash
ls -la /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/
readlink -f /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so
```

Ожидаемый целевой файл:

```text
/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
```

## RUNPATH

```bash
patchelf --print-rpath /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
```

Ожидается `/opt/elan-fingerprint/lib`.

## Зависимости

```bash
ldd /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
```

Строк `not found` быть не должно. В частности, `libcrypto.so.1.1` должна разрешаться в `/opt/elan-fingerprint/lib/libcrypto.so.1.1`.

Одновременное наличие системной `libcrypto.so.3` через транзитивные зависимости само по себе не является ошибкой.

## Проверить фактически загруженные библиотеки

Когда `fprintd` работает:

```bash
pid=$(pidof fprintd)
sudo grep -E 'elan-fingerprint|libcrypto\.so\.1\.1' "/proc/$pid/maps"
```

Ожидаются пути из `/opt/elan-fingerprint`.

## Регистрация и проверка отпечатка

```bash
fprintd-list "$USER"
fprintd-verify
```

Рабочее устройство называется `ELAN Fingerprint Sensor (press)`. Если вместо него используется стандартный открытый драйвер с устройством `ElanTech Fingerprint Sensor`, проверьте символическую ссылку TOD, правило udev и журнал.

## Восстановление конфигурации PAM

Если аутентификация по отпечатку пальца работает неправильно, но `sudo` ещё доступен:

```bash
sudo pam-auth-update
```

Отключите **Fingerprint authentication**, затем выполните:

```bash
sudo -k
sudo -v
```

Если ранее были сохранены резервные копии PAM, не восстанавливайте их механически после других изменений системы: сначала сравните резервную копию с текущими файлами PAM.

## LED

Отсутствие мигания LED не означает, что сканер отпечатков не работает. Управление LED не входит в текущий пакет.
