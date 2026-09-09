# Troubleshooting

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

Ожидаемый target:

```text
/opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
```

## RUNPATH

```bash
patchelf --print-rpath /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
```

Ожидается `/opt/elan-fingerprint/lib`.

## Dependencies

```bash
ldd /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
```

`not found` быть не должно. В частности, `libcrypto.so.1.1` должен разрешаться в `/opt/elan-fingerprint/lib/libcrypto.so.1.1`.

Наличие одновременно системного `libcrypto.so.3` через транзитивные dependencies само по себе не является ошибкой.

## Проверить реально загруженные библиотеки

Когда `fprintd` работает:

```bash
pid=$(pidof fprintd)
sudo grep -E 'elan-fingerprint|libcrypto\.so\.1\.1' "/proc/$pid/maps"
```

Ожидаются пути из `/opt/elan-fingerprint`.

## Enrollment и verify

```bash
fprintd-list "$USER"
fprintd-verify
```

Рабочее устройство называется `ELAN Fingerprint Sensor (press)`. Если вместо него используется stock/open-source path `ElanTech Fingerprint Sensor`, проверьте TOD symlink, udev rule и journal.

## PAM recovery

Если fingerprint authentication ведёт себя неправильно, но `sudo` ещё доступен:

```bash
sudo pam-auth-update
```

Отключите **Fingerprint authentication**, затем:

```bash
sudo -k
sudo -v
```

Если ранее были сохранены PAM backups, не восстанавливайте их механически после других изменений системы: сначала сравните backup с текущими PAM files.

## LED

Отсутствие мигания LED не означает, что fingerprint sensor не работает. LED control не является частью текущего package.
