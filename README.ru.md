# Поддержка ELAN 04f3:0c4b в Ubuntu 26.04

[English](README.md) | [Русский](README.ru.md)

Локальный Debian-пакет и документация для USB-сканера отпечатков пальцев **ELAN 04f3:0c4b**, проверенные на **Lenovo ThinkBook 14 G2 ARE** под **Ubuntu 26.04 Resolute**.

> [!IMPORTANT]
> Это не официальный пакет Ubuntu, Lenovo или ELAN. В состав `.deb` входят сторонний проприетарный бинарный модуль ELAN TOD и изолированная библиотека совместимости `libcrypto.so.1.1`. См. [`THIRD_PARTY_NOTICES.ru.md`](THIRD_PARTY_NOTICES.ru.md).

## Проверенная конфигурация

- Оборудование: Lenovo ThinkBook 14 G2 ARE
- USB ID: `04f3:0c4b Elan Microelectronics Corp. ELAN:Fingerprint`
- Ubuntu: 26.04 Resolute
- Ядро: `7.0.0-31-generic`
- `fprintd`: `1.94.5-4`
- `libfprint-2-2`: `1:1.95.1+tod1-0ubuntu2`
- `libfprint-2-tod1`: `1:1.95.1+tod1-0ubuntu2`
- `libpam-fprintd`: `1.94.5-4`
- Локальный пакет: `elan-fingerprint-04f3-0c4b-local 1.0.0+local1`

Успешно проверены регистрация отпечатка, `fprintd-verify`, `sudo`, экран блокировки GNOME, графический вход в систему и резервная аутентификация паролем.

## Архитектура

```text
ELAN USB 04f3:0c4b
  -> правило udev
  -> libfprint TOD
  -> /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
  -> RUNPATH=/opt/elan-fingerprint/lib
  -> изолированная libcrypto.so.1.1
  -> fprintd -> pam_fprintd -> sudo / экран блокировки / вход в систему
```

Устаревшая версия OpenSSL **не устанавливается глобально**. Проект не создаёт ссылку `libcrypto.so.1.1 -> libcrypto.so.3`, не изменяет `ld.so.conf`, не выполняет глобальные изменения через `ldconfig` и не задаёт глобальный `LD_LIBRARY_PATH`.

## Установка

Проверьте наличие устройства:

```bash
lsusb | grep -i '04f3:0c4b'
```

Проверьте SHA256 скачанного `.deb`:

```bash
sha256sum elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb
```

Ожидаемая контрольная сумма:

```text
9893258dfeb04259312fccb465ac372440e2e972787966794e63b87c9c520b44
```

Установите пакет:

```bash
sudo dpkg -i ./elan-fingerprint-04f3-0c4b-local_1.0.0+local1_amd64.deb
```

Пакет устанавливает драйвер TOD, изолированную `libcrypto.so.1.1`, символическую ссылку на драйвер TOD и правило udev. **Конфигурацию PAM пакет не изменяет.**

## Проверка

```bash
patchelf --print-rpath /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so
ldd /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/libfprint-2-tod1-elan.so | grep -E 'crypto|not found'
sha256sum -c /opt/elan-fingerprint/docs/SHA256SUMS
fprintd-list "$USER"
fprintd-verify
```

Рабочий сканер определяется как `ELAN Fingerprint Sensor (press)`, а проверка завершается сообщением `Verify result: verify-match (done)`.

Если отпечаток ещё не зарегистрирован:

```bash
fprintd-enroll -f right-index-finger "$USER"
```

## PAM

Перед включением аутентификации по отпечатку убедитесь, что обычный пароль известен и работает:

```bash
sudo pam-auth-update
```

Включите **Fingerprint authentication**, сохранив существующие способы аутентификации по паролю. Сначала проверьте вход по отпечатку командой `sudo -k; sudo -v`. Затем отдельным тестом дождитесь истечения времени ожидания отпечатка и убедитесь, что вход по паролю по-прежнему работает. Только после этого проверяйте экран блокировки и выход с последующим входом в систему.

## Безопасное полное удаление

**Не удаляйте драйвер первым, если PAM всё ещё ожидает аутентификацию по отпечатку.**

1. Убедитесь, что знаете рабочий пароль.
2. Выполните `sudo pam-auth-update`.
3. Отключите **Fingerprint authentication**.
4. Выполните `sudo -k && sudo -v` и подтвердите вход паролем.
5. Желательно также проверить разблокировку экрана паролем.
6. Если вы полностью отказываетесь от биометрии, пока драйвер ещё работает, можно выполнить `fprintd-delete "$USER"`.
7. Удалите пакет вместе с его конфигурацией:

```bash
sudo apt purge elan-fingerprint-04f3-0c4b-local
```

8. Проверьте файлы, принадлежащие пакету. Локальные резервные файлы в `/opt/elan-fingerprint/docs` могут остаться, поскольку пакет ими не владеет. Не удаляйте весь каталог `/opt/elan-fingerprint` вслепую.

Полная процедура: [`docs/ru/UNINSTALL.md`](docs/ru/UNINSTALL.md).

## Светодиод

Драйвер TOD обеспечивает сканирование отпечатков, но светодиод кнопки питания может не мигать так, как в Windows. Поддержка светодиода намеренно не входит в этот пакет и рассматривается отдельно.

## Не рекомендуется

- устанавливать устаревший `libssl1.1` глобально только ради этого драйвера;
- создавать ссылку `libcrypto.so.1.1 -> libcrypto.so.3`;
- добавлять изолированную библиотеку OpenSSL из этого пакета в `/etc/ld.so.conf*`;
- задавать глобальный `LD_LIBRARY_PATH`;
- подменять выпуск Ubuntu ради старого PPA;
- удалять системный стек libfprint;
- удалять драйвер до проверки аутентификации по паролю.

## Документация

- [`docs/ru/PACKAGE.md`](docs/ru/PACKAGE.md) — устройство пакета и проверки.
- [`docs/ru/UNINSTALL.md`](docs/ru/UNINSTALL.md) — безопасное полное удаление.
- [`docs/ru/TROUBLESHOOTING.md`](docs/ru/TROUBLESHOOTING.md) — диагностика.
- [`THIRD_PARTY_NOTICES.ru.md`](THIRD_PARTY_NOTICES.ru.md) — происхождение и лицензирование сторонних компонентов.

Канонические версии документации на английском языке: [`README.md`](README.md), [`docs/en/`](docs/en/) и [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md).

## Лицензия

Материалы, созданные специально для этого репозитория, включая оригинальную документацию, метаданные пакета, сценарии сопровождения и другой оригинальный код проекта, предоставляются по **лицензии Apache 2.0**. См. [`LICENSE`](LICENSE).

Это **не означает**, что Apache 2.0 распространяется на сторонние бинарные компоненты. В частности, проприетарный драйвер ELAN TOD и `libcrypto.so.1.1` сохраняют собственные применимые условия лицензирования и отдельно описаны в [`THIRD_PARTY_NOTICES.ru.md`](THIRD_PARTY_NOTICES.ru.md).

Для `libcrypto.so.1.1` в [`THIRD_PARTY_NOTICES.ru.md`](THIRD_PARTY_NOTICES.ru.md) теперь зафиксированы точный пакет Ubuntu, контрольные суммы, соответствующий пакет с исходным кодом и применимые лицензионные условия OpenSSL 1.1.1f. Полный текст лицензии и сведения о происхождении также включены в дерево пакета.

До подтверждения прав на распространение бинарного модуля ELAN TOD наличие его копии или пакета в этом репозитории не следует трактовать как предоставление каких-либо прав на этот компонент со стороны автора репозитория.
