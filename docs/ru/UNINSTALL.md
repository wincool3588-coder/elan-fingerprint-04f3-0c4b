# Безопасное полное удаление

[English](../en/UNINSTALL.md) | [Русский](UNINSTALL.md)

Эта процедура предназначена для системы, где аутентификация по отпечатку пальца уже включена в PAM.

## 1. Сначала отключить аутентификацию по отпечатку пальца в PAM

Убедитесь, что знаете рабочий пароль, затем выполните:

```bash
sudo pam-auth-update
```

Отключите **Fingerprint authentication**. Не отключайте обычную аутентификацию Unix по паролю.

## 2. Убедиться, что аутентификация по паролю работает

```bash
sudo -k
sudo -v
```

Аутентификация должна успешно пройти с паролем. Желательно также заблокировать экран и проверить вход по паролю.

**Не переходите к удалению драйвера, пока этот тест не пройден.**

## 3. При необходимости удалить зарегистрированные отпечатки

Если требуется полностью удалить биометрические данные, сделайте это, пока драйвер ещё работает:

```bash
fprintd-delete "$USER"
fprintd-list "$USER"
```

Если требуется только отключить аутентификацию по отпечатку пальца, удалять зарегистрированные отпечатки необязательно.

## 4. Удалить пакет

```bash
sudo apt purge elan-fingerprint-04f3-0c4b-local
```

Альтернатива без `purge`:

```bash
sudo dpkg -r elan-fingerprint-04f3-0c4b-local
```

`purge` предпочтительнее для полного удаления, поскольку правило udev является conffile.

## 5. Проверить результат

```bash
dpkg-query -W elan-fingerprint-04f3-0c4b-local
ls -la /usr/lib/x86_64-linux-gnu/libfprint-2/tod-1/
test ! -e /etc/udev/rules.d/60-libfprint-2-tod1-elan.rules && echo 'udev rule removed'
test ! -e /opt/elan-fingerprint/driver/libfprint-2-tod1-elan.so && echo 'driver removed'
test ! -e /opt/elan-fingerprint/lib/libcrypto.so.1.1 && echo 'private libcrypto removed'
```

## 6. Локальные резервные файлы

До создания пакета в `/opt/elan-fingerprint/docs` могли быть созданы локальные файлы, например `working-config.txt`, `ROLLBACK.txt`, `PAM-ROLLBACK.txt` и каталог `pam-before/`. Они не обязательно принадлежат пакету и могут остаться после `purge`.

Проверьте дерево файлов:

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
 -> проверить sudo с паролем
 -> проверить экран блокировки с паролем
 -> при необходимости удалить зарегистрированные отпечатки
 -> apt purge elan-fingerprint-04f3-0c4b-local
 -> проверить оставшиеся файлы
 -> отдельно решить, что делать с локальными резервными файлами
```
