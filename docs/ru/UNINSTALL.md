# Безопасное полное удаление

[English](../en/UNINSTALL.md) | [Русский](UNINSTALL.md)

Эта процедура предназначена для системы, где fingerprint authentication уже включён в PAM.

## 1. Сначала отключить fingerprint authentication в PAM

Убедитесь, что знаете рабочий пароль, затем:

```bash
sudo pam-auth-update
```

Снимите **Fingerprint authentication**. Не отключайте обычную Unix password authentication.

## 2. Доказать, что password authentication работает

```bash
sudo -k
sudo -v
```

Аутентификация должна пройти паролем. Желательно также заблокировать экран и проверить password login.

**Не переходите к удалению driver, пока этот тест не пройден.**

## 3. Опционально удалить enrolled fingerprints

Если требуется полностью отказаться от биометрических данных, пока driver ещё работает:

```bash
fprintd-delete "$USER"
fprintd-list "$USER"
```

Если требуется только отключить fingerprint authentication, enrollment удалять необязательно.

## 4. Purge package

```bash
sudo apt purge elan-fingerprint-04f3-0c4b-local
```

Альтернатива без purge:

```bash
sudo dpkg -r elan-fingerprint-04f3-0c4b-local
```

`purge` предпочтительнее для полного удаления, поскольку udev rule является conffile.

## 5. Проверить результат

```bash
dpkg-query -W elan-fingerprint-04f3-0c4b-local
ls -la /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/
test ! -e /etc/udev/rules.d/60-libfprint-2-tod1-elan.rules && echo 'udev rule removed'
test ! -e /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so && echo 'driver removed'
test ! -e /opt/elan-fingerprint/lib/libcrypto.so.1.1 && echo 'private libcrypto removed'
```

## 6. Локальные backup-файлы

До packaging в `/opt/elan-fingerprint/docs` могли быть созданы локальные файлы, например `working-config.txt`, `ROLLBACK.txt`, `PAM-ROLLBACK.txt` и `pam-before/`. Они не обязательно принадлежат package и после purge могут остаться.

Проверьте:

```bash
find /opt/elan-fingerprint -maxdepth 3 -ls
```

Только если там не осталось ничего нужного:

```bash
sudo rm -rf /opt/elan-fingerprint
```

Никогда не выполняйте эту последнюю команду вслепую.

## Правильный порядок

```text
рабочий пароль
 -> отключить Fingerprint authentication в pam-auth-update
 -> проверить sudo по паролю
 -> проверить lock screen по паролю
 -> при необходимости удалить enrollment
 -> apt purge elan-fingerprint-04f3-0c4b-local
 -> проверить остатки
 -> отдельно решить судьбу локальных backup-файлов
```
