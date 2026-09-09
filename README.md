# ELAN 04f3:0c4b fingerprint support for Ubuntu 26.04

Локальный Debian-пакет и документация для USB fingerprint reader **ELAN 04f3:0c4b**, проверенные на **Lenovo ThinkBook 14 G2 ARE** под **Ubuntu 26.04 Resolute**.

> [!IMPORTANT]
> Это не официальный пакет Ubuntu, Lenovo или ELAN. В состав `.deb` входит сторонний proprietary ELAN TOD blob и приватная compatibility-библиотека `libcrypto.so.1.1`. См. `THIRD_PARTY_NOTICES.md`.

## Проверенная конфигурация

- Hardware: Lenovo ThinkBook 14 G2 ARE
- USB ID: `04f3:0c4b Elan Microelectronics Corp. ELAN:Fingerprint`
- Ubuntu: 26.04 Resolute
- Kernel: `7.0.0-31-generic`
- `fprintd`: `1.94.5-4`
- `libfprint-2-2`: `1:1.95.1+tod1-0ubuntu2`
- `libfprint-2-tod1`: `1:1.95.1+tod1-0ubuntu2`
- `libpam-fprintd`: `1.94.5-4`
- Local package: `elan-fingerprint-04f3-0c4b-local 1.0.0+local1`

Успешно проверены enrollment, `fprintd-verify`, `sudo`, GNOME lock screen, graphical login и password fallback.

## Архитектура

```text
ELAN USB 04f3:0c4b
  -> udev rule
  -> libfprint TOD
  -> /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
  -> RUNPATH=/opt/elan-fingerprint/lib
  -> private libcrypto.so.1.1
  -> fprintd -> pam_fprintd -> sudo / lock screen / login
```

Старый OpenSSL **не устанавливается глобально**. Не создаётся `libcrypto.so.1.1 -> libcrypto.so.3`, не меняются `ld.so.conf`, `ldconfig` и глобальный `LD_LIBRARY_PATH`.

## Установка

Проверьте устройство:

```bash
lsusb | grep -i '04f3:0c4b'
```

Проверьте SHA256 скачанного `.deb`:

```bash
sha256sum elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb
```

Ожидается:

```text
9893258dfeb04259312fccb465ac372440e2e972787966794e63b87c9c520b44
```

Установка:

```bash
sudo dpkg -i ./elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb
```

Пакет устанавливает TOD driver, private `libcrypto.so.1.1`, TOD symlink и udev rule. **PAM пакет не изменяет.**

## Проверка

```bash
patchelf --print-rpath /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
ldd /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so | grep -E 'crypto|not found'
sha256sum -c /opt/elan-fingerprint/docs/SHA256SUMS
fprintd-list "$USER"
fprintd-verify
```

Рабочий sensor определяется как `ELAN Fingerprint Sensor (press)`, а verify заканчивается `Verify result: verify-match (done)`.

Если enrollment отсутствует:

```bash
fprintd-enroll -f right-index-finger "$USER"
```

## PAM

Перед включением fingerprint убедитесь, что обычный пароль известен и работает:

```bash
sudo pam-auth-update
```

Включите **Fingerprint authentication**, сохранив существующие password methods. Затем сначала проверьте fingerprint через `sudo -k; sudo -v`, а отдельным тестом дождитесь fingerprint timeout и убедитесь, что password fallback работает. Только после этого тестируйте lock screen и logout/login.

## Правильное полное удаление

**Не удаляйте driver первым, если PAM всё ещё ожидает fingerprint.**

1. Убедитесь, что знаете рабочий пароль.
2. `sudo pam-auth-update`.
3. Отключите **Fingerprint authentication**.
4. Выполните `sudo -k && sudo -v` и подтвердите вход паролем.
5. Желательно проверить lock screen с паролем.
6. При полном отказе от биометрии, пока driver ещё работает, можно выполнить `fprintd-delete "$USER"`.
7. Удалите package:

```bash
sudo apt purge elan-fingerprint-04f3-0c4b-local
```

8. Проверьте package-owned files. Локальные backup-файлы в `/opt/elan-fingerprint/docs` могут остаться, поскольку package ими не владеет. Не удаляйте весь `/opt/elan-fingerprint` вслепую.

Полная процедура: [`docs/UNINSTALL.md`](docs/UNINSTALL.md).

## LED

TOD driver обеспечивает fingerprint scanning, но LED power button может не мигать как в Windows. LED helper намеренно не включён в package и рассматривается отдельно.

## Не делать

- не устанавливать старый `libssl1.1` глобально ради driver;
- не создавать `libcrypto.so.1.1 -> libcrypto.so.3`;
- не добавлять private OpenSSL в `/etc/ld.so.conf*`;
- не задавать глобальный `LD_LIBRARY_PATH`;
- не подменять Ubuntu suite для старого PPA;
- не удалять системный libfprint;
- не удалять driver до проверки password authentication.

## Документация

- [`docs/PACKAGE.md`](docs/PACKAGE.md) — устройство package и проверки.
- [`docs/UNINSTALL.md`](docs/UNINSTALL.md) — безопасное полное удаление.
- [`docs/TROUBLESHOOTING.md`](docs/TROUBLESHOOTING.md) — диагностика.
- [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) — происхождение и лицензирование сторонних компонентов.
