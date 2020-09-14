---
title: 'Проблемы, специфичные для macOS 11 Big Sur'
taxonomy:
    category:
        - docs
visible: true
---

Выход следующей версии macOS, **BigSur**, намечен на конец 2020 года, но бета-версия уже доступна. И BigSur уже обуславливает проблемы в работе многих приложений, AdGuard не оказался исключением. В этой статье собраны известные проблемы и возможные способы их решения.

* [Совместимость с локальными прокси](#local-proxies)
* [Входящие соединения](#accepting-connections)
* [Использование режима "Автоматический прокси"](#automatic-proxy)
* [Решение проблем с SSH](#ssh)
* [Переключение на Kernel Extension в Big Sur](#kernel-extension)


<a htef="local-proxies"></a>

## Совместимость с локальными прокси

Чтобы настроить вышестоящий прокси в AdGuard для Mac на Big Sur, вам необходимо перейти в *Меню AdGuard -> Дополнительно -> Расширенные настройки...*. Кликните по области *Значение* настройки `upstream.proxy`, чтобы настроить прокси.

<img src="https://cdn.adguard.com/public/Adguard/kb/BigSur/problems/proxy_en.png" style="border: 1px solid #efefef; max-width: 500px; padding: 2px;">

Введите строку вида `scheme://user:passwrod@host:port`, где

* `scheme` имеет значение `http`, `https`, `socks4` или `socks5`, в зависимости от типа прокси.

>Если вы используете тип прокси `socks5`, установите значение настройки `upstream.proxy.socks5udp` как `true`, чтобы указать AdGuard направлять UDP-трафик через прокси-сервер.

* `user` и `password` — имя пользователя и пароль от вашего прокси соответственно (если требуются). Игнорируйте эти параметры, если один из них (или оба) не применимы к данному прокси.
* `host` — IP-адрес вашего прокси-сервера. 
* `port` — желаемый номер порта, который будет использоваться прокси-сервером.

>Пример: стркоа `socks5://localhost:6322` настроит локальный прокси типа SOCKS5, который слушает порт 6322 и не требует имени пользователя и пароля.

Нажмите *Применить*, чтобы заставить AdGuard пересылать весь трафик через настроенный прокси-сервер.


<a href="accepting-connections"></a>

## Входящие соединения

На Big Sur любой сервер из сети, фильтруемой AdGuard, не сможет подключиться к вашему Mac, если вы используете его как сервер (по умолчанию AdGuard фильтрует весь трафик, кроме localhost). Чтобы избежать этой проблемы, вам необходимо добавить в исключения адреса host-серверов (или даже целые подсети), с которых идёт соединение.

Перейдите в *Меню AdGuard -> Дополнительно -> Расширенные Настройки...* и кликните по области *Значение* настройки `network.extension.route.exclude`.

<img src="https://cdn.adguard.com/public/Adguard/kb/BigSur/problems/connections_en.png" style="border: 1px solid #efefef; max-width: 500px; padding: 2px;">

Введите здесь любые IP-адреса и подсети, разделяя их запятыми и переносами строки.


<a href="automatic-proxy"></a>

## Использование режиме "Автоматический прокси"

Если вы столкнулись в какими-либо проблемами в Big Sur, которые нельзя решить способами, описанными выше, вы можете попробовать переключить AdGuard в режим *Автоматический прокси*.

1) Откройте меню AdGuard.
2) Выберите *Настройки...*. 
3) Переключитесь на вкладку *Сеть*. 
4) Кликните по кнопке *Выбрать режим...*.
5) Выберите *Автоматический прокси*.

<img src="https://cdn.adguard.com/public/Adguard/kb/BigSur/problems/automatic-proxy_en.png" style="border: 1px solid #efefef; max-width: 500px; padding: 2px;">

AdGuard автоматически добавил **.pac**-файл в сетевые настройки вашего Mac, и теперь система будет распознавать AdGuard как прокси и попытается направлять через него весь трафик.

>Примите во внимание, что некоторые приложения могут игнорировать это настройку, и в таком случае их трафик не будет фильтроваться.


<a href="kernel-extension"></a>

## Переключение на Kernel Extension в Big Sur

По умолчанию, на Big Sur AdGuard использует фреймворк Network Extension, так как используемый ранее фреймворк Kernel Extension отключён в новой версии системы. Это может вызывать проблемы совместимости, но чтобы включить Kernel Extension обратно, вам сначала потребуется отключить системную настройку безопасности System Integrity Protection (SIP). Для отключения SIP следуйте этой инструкции:

1) Кликните по *Символу Apple* в строке меню.
2) Кликните *Перезапуск…*
3) Зажмите *Command-R*, чтобы запустить систему в Режиме Восстановления.
4) Кликните по кнопке *Утилиты*.
5) Выберите *Терминал*.
6) Вбейте в появившемся окне `csrutil disable`.
7) Нажмите на клавиатуре клавишу *Return* или *Enter*.
8) Кликните по *Символу Apple* в строке меню.
9) Кликните *Перезапуск…*

Теперь, когда SIP отключён, выполните следущие шаги для включения Kernel Extension:

<img src="https://cdn.adguard.com/public/Adguard/kb/BigSur/problems/kernel_en.png" style="border: 1px solid #efefef; max-width: 500px; padding: 2px;">

1) Откройте меню AdGuard.
2) Выберите *Настройки...*. 
3) Переключитесь на вкладку *Сеть*. 
4) Кликните по кнопке *Выбрать режим...*.
5) Выберите *Kernel Extension*.

>Однако, мы рекомендуем использовать этот метод только в том случае, если все остальные не дали результата, посколько он может привести к непредвиденным последствиям.


<a href="ssh"></a>

## Решение проблем с SSH

Фреймворк Network Extension, который AdGuard вынужден испольщовать в Big Sur, конфликтует с некоторыми другими фреймворками от Apple, такими как Network (не путать с *Network Extension*). В результате этого любой процесс, треубющий использование этих библиотек, может работать некорректно при активном AdGuard. Одна из таких проблем — подключение к удалённому серверу через SSH.

Для решения этой проблемы, вы можете перейти на [HomeBrew SSH](https://formulae.brew.sh/formula/openssh), который не использует проблемный фреймворк Network.

Также вы можете выбрать другой режим фильтрации в AdGuard: [Автоматический прокси](#automatic-proxy) или [Kernel Extension](#kernel-extension).