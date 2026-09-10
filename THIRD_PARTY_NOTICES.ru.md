# Уведомления о сторонних компонентах

[English](THIRD_PARTY_NOTICES.md) | [Русский](THIRD_PARTY_NOTICES.ru.md)

Этот репозиторий документирует и упаковывает сторонние компоненты, необходимые для работы fingerprint reader ELAN `04f3:0c4b` со стеком Ubuntu libfprint TOD.

## Область действия лицензии репозитория

Apache License 2.0 в [`LICENSE`](LICENSE) применяется только к материалам, созданным специально для этого репозитория: оригинальной документации, packaging metadata, maintainer scripts и другому оригинальному коду проекта.

Она **не перелицензирует** стороннее программное обеспечение или бинарные компоненты. Каждый сторонний компонент остаётся под собственными условиями copyright и лицензирования. Наличие, упоминание или упаковка стороннего компонента не должны трактоваться как предоставление прав сверх тех, которые предоставлены правообладателем или применимой лицензией.

## ELAN TOD binary

`libfprint-2-tod1-elan.so` — proprietary/redistributed binary, связанный с поддержкой fingerprint ELAN/Lenovo. Он **не создан этим репозиторием**, не покрывается Apache-2.0 этого репозитория и не заявляется как open-source.

Рабочий binary, используемый в package `1.0.0+local1`, имеет SHA256:

```text
be47d4e63bdb541397d1a0d853115d034c36179580aade16e3b52bdcd1dbc3f0
```

Source/reference, использованные при восстановлении работоспособности: репозиторий TonyHoyle `libfprint-2-tod1-elan` и device-specific guide `Abishek-Pechiappan/libfprint-elan-04f3-0c4b-tod`.

**Статус перераспространения:** не определён. Права на перераспространение proprietary blob не устанавливаются документацией или Apache-2.0 этого репозитория. Любой, кто распространяет binary, должен самостоятельно проверить применимые условия Lenovo/ELAN. Пока этот вопрос не решён, наличие копии binary в репозитории или в собранном package не следует трактовать как утверждение о наличии разрешения на распространение.

## Compatibility library OpenSSL 1.1

Package содержит только `libcrypto.so.1.1`, изолированную в `/opt/elan-fingerprint/lib` и выбираемую private RUNPATH ELAN driver. Она извлечена из официального amd64 package Ubuntu 20.04 LTS (Focal):

```text
libssl1.1_1.1.1f-1ubuntu2.24_amd64.deb
SHA256: 7cf39d70a639017d1dd7c8d36daa2258063608688e449fddf40ffdd46f992a78
```

Bundled library имеет SHA256:

```text
bf99926de2ce739d3fdccc4a551d97d64aac6e968589aaad9dc4ac8d143b519a
```

SHA256 bundled library был отдельно проверен после извлечения из указанного Ubuntu binary package.

Соответствующий Ubuntu source package:

```text
openssl 1.1.1f-1ubuntu2.24
```

Ubuntu публикует соответствующие source materials:

```text
openssl_1.1.1f.orig.tar.gz
SHA256: 186c6bfe6ecfba7a5b48c47f8a1673d0f3b0e5ba2e25602dd23b629975da3f35

openssl_1.1.1f-1ubuntu2.24.debian.tar.xz
SHA256: 66b1a31642710d386b6896e2e7bea0bd3138d94277c894f871ccaa52bad07c04

openssl_1.1.1f-1ubuntu2.24.dsc
SHA256: f9b93b532511ee24b3e0160c0c7549d3e3123c9e2d9c5c6da0e6de2f582eccd3
```

OpenSSL 1.1.1f распространяется по **OpenSSL License и Original SSLeay License; применяются обе лицензии**. Эти лицензии разрешают binary redistribution при сохранении соответствующих copyright notices, attribution и disclaimer. OpenSSL остаётся сторонним программным обеспечением и **не перелицензируется** под Apache-2.0 этого репозитория.

Package содержит полный upstream license text OpenSSL 1.1.1f и сведения о provenance по путям:

```text
/usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/LICENSE
/usr/share/doc/elan-fingerprint-04f3-0c4b-local/third-party/openssl/SOURCE
```

В этих материалах сохранены требуемые acknowledgements, включая attribution OpenSSL Project и Eric Young.

Официальная информация о source package: <https://launchpad.net/ubuntu/+source/openssl/1.1.1f-1ubuntu2.24>

Upstream license source: <https://github.com/openssl/openssl/blob/OpenSSL_1_1_1f/LICENSE>

## Документация и packaging репозитория

Оригинальная документация, packaging metadata, maintainer scripts и другие материалы, созданные специально для этого репозитория, лицензируются по Apache License 2.0, если отдельный файл явно не указывает иное.

Эти условия не изменяют, не заменяют и не отменяют лицензии или ограничения на распространение bundled или referenced third-party components.

## Отслеживание юридического статуса

Оставшиеся вопросы по перераспределению отслеживаются в GitHub Issue #1. Для OpenSSL 1.1 compatibility component теперь документированы provenance, соответствующий source package и bundled license notices. Статус proprietary ELAN TOD binary остаётся не определённым, поэтому release artifacts, содержащие его, по-прежнему следует считать имеющими неопределённый third-party redistribution status.
