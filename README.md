Vozovoz API 2.5
===============

[English version](en/README.md)

Vozovoz API представляет интерфейс для взаимодействия с данными https://vozovoz.ru. В качестве протокола передачи данных используется HTTPS.
Протестировать взаимодействия с API можно на [демо-сервере](https://vozovoz.org/dev/api/).

## Содержание

* [Быстрый старт](ru/docs/quick.md)
    * [Расчёт стоимости заказа](ru/docs/object/price.md)
    * [Оформление заказа](ru/docs/object/order.md#set)
* [Параметры запросов](ru/docs/params/index.md)
    * [Авторизация](ru/docs/params/auth.md)
* [Объекты](ru/docs/object/index.md)
    * [Локация](ru/docs/object/location.md)
    * [Заказ](ru/docs/object/order.md)
        * [Получить](ru/docs/object/order.md#get)
        * [Оформить](ru/docs/object/order.md#set)
    * [Прямой запрос](ru/docs/object/directQuery.md)
    * [Расписание дат доставки](ru/docs/object/schedule.md)
    * [Стоимость](ru/docs/object/price.md)
    * [Терминал](ru/docs/object/terminal.md)
    * [Упаковка](ru/docs/object/wrapping.md)
* [Структуры](ru/docs/structure/index.md)
    * [Груз](ru/docs/structure/cargo.md)
        * [Габариты](ru/docs/structure/cargo.md#dimension)
        * [Упаковка](ru/docs/structure/cargo.md#wrapping)
    * [Контрагент](ru/docs/structure/customer.md)
    * [Пункт доступа](ru/docs/structure/gateway.md)
        * [Точка доступа](ru/docs/structure/gateway.md#point)
        * [Услуги](ru/docs/structure/service.md)

***

По всем вопросам обращайтесь на почту [api@vozovoz.ru](mailto:api@vozovoz.ru).