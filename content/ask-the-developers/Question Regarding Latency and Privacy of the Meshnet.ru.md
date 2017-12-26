+++
title = "Вопрос относительно задержки и конфиденциальности mesh сети"
tags = [
    "Ask the Developers",
    "Skywire",
    "Meshnet",
    "Privacy",
]
bounty = 4
date = "2017-08-18"
categories = [
    "Skywire",
    "Ask the Developers",
]
+++

*Этот пост относится к Skywire в раннем своем представлении, прежде чем он был назван Skywire. Адаптированный пост с bitcointalk от 09 августа 2014 года.*

Цитата пользователя: **CraigM** 9 августа, 2014, 07:20:24 

>Mesh сеть предназначена для того, чтобы быть хорошим инструментом конфиденциальности с преимуществами, сравнимыми с Тором, но с меньшей задержкой, правильно?

>Правильно ли, что mesh сеть позволит финансировать узлы (ноды) через микроплатежи посредством Skycoin, чтобы покрыть затраты на пропускную способность? Не приводит ли это к тому, что все операторы узлов записывают подробные и публичные логи (по их личным блокчейнам), описывающие все транзакции, которые аналогичны каждому, кто отправляет данные через свой узел? Это похоже на то, что любая третья сторона может совершать атаки посредством корреляции трафика как в Торе, за исключением того, что вам не нужен доступ к соединениям. Даже если они не станут публично контролируемыми, ведение полных логов может иметь реальные проблемы (они занимают много места и также их могут запросить правоохранительные органы).

>Первоначальная версия будет поставляться с централизованным сервером поиска маршрутов, правильно? Это означает, что если вы хотите подключиться к кому-то, вы должны сообщить об этом третьей стороне, так? Кажется, что это непохоже на Тор с его службой конфиденциальности, пока это не будет исправлено. Есть ли основания полагать, что вы решите эту проблему в ближайшее время (или когда-либо: потому что это трудно)?

>Как вы найдете путь к этой доверенной третьей стороне, которая будет выполнять поиск маршрута для вас? Я предполагаю, вы будете делать это как особый случай (не используйте выбор маршрутизации на стороне отправителя), но мне любопытно, может у вас есть другой вариант.

## Ответ

Да. Это на самом деле быстрее TCP/IP. Интернет-провайдеры используют метод "горячей картошки" при выборе маршрута. Задержка не должна быть хуже TCP / IP, и теоретически может быть быстрее.

**Гарантия конфиденциальности**

- Каждый узел знает только откуда пришел и куда направить сигнал.
- Информация, которая передается между узлами зашифрована.
- Передача использует оконечное (end-to-end) шифрование.
- Ваша передача защищена от активного вмешательства в соединение посредством использования открытых ключей для конечных точек узла (сетевые адреса являются открытыми ключами в сети).
- Все шифрование эфемерно и использует технологию отрицаемого шифрования.
- Протокол предназначен для предотвращения попыток глубокой проверки пакетов для идентификации пользователей, выполняющих протокол (нет фиксированных портов, подключения фиксированного узла для магистрали и т. д.).

Таким образом, это очень походит на Тор с низкой задержкой и с микроплатежами для пропускной способоности.

- Она более безопасна и имеет более высокую конфиденциальность, чем HTTPS.
- Она быстрее Тора и других масштабных программ, но пока может быть атакована.
- Код намного проще, чем у Тора, поэтому остается меньше места для бэкдоров или скрытых уязвимостей. Во всей реализации есть только одна внешняя зависимость.
- Если вам нужна абсолютная защита от временны́х атак, вы должны использовать службу микширования или запустить Bitmessage поверх даркнета.
- Все сети с низкой задержкой подвергаются временны́м атакам.


## Маршрутные серверы

Yes route servers are a weak link. For maximum privacy you should run your own internal route server.

However, if you do use a public route server, you are connected to it through several hops, so it cannot identify you. It would still be safer to run your own.

## Handling of Micropayments for Bandwidth

The way micropayments are handled, is through a third party. The node connects to a "**gateway**", deposits a coin with the gateway to get a credit. The node can now generate pseudonymous 64 bit "addresses" with the gateway. The gateway does not know the identity of the connecting node. It only knows the previous hop the connection came through.

So if you establish twelve connections to the gateway, they look like twelve different users to the gateway. Eventually communication to the gateway will be over an asynchronous messaging channel.

The node forwarding the bandwidth, connects to the gateway also. The two nodes can now pay each other through the gateway, without the gateway knowing the identity of either of the two nodes. When a node has enough coins in credit (a full coin), it can generate a new Skycoin address and withdrawal the coin to that address. Gateways are only handling small amounts of coins

A gateway in the Skycoin protocol is any server that holds coins or account balances on behalf of 3rd parties. Gateways are deposit institutions and they have their own protocol and API.

**Eventually,**

- there will be multiple gateways and cross gateway coin transfers. These transactions occur in private and do not appear on the blockchain until you withdraw the coins from the gateway.
- messaging with gateway will occur through an asynchronous communication channel (each message steam will get a new pseudonymous identity)
- part of the gateway protocol is an OT implementation, which allows you to prove if a particular gateway is stealing coins. You sign each API call to the gateway, then gateway executes and signs the output. So there is a chain of linked signatures and transactions and the gateway cannot make coins disappear without being able to forge your signature. If you deposit coins somewhere and they disappear, you can publish your transaction log and then the owner of the accused node has to produce a log showing that you authorized the coins to go somewhere. If they cannot produce a signed API call, then it proves they are lying/dishonest.
- Eventually exchanges will operate under the gateway protocol

Your suggestion of having a public blockchain for the internal balances in the gateway is interesting. I think putting the internal transactions on a public personal block chain, could keep exchanges honest while still maintaining user privacy.
