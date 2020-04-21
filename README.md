Vozovoz API 2.0
===============

Vozovoz API представляет интерфейс для взаимодействия с данными https://vozovoz.ru/. В качестве протокола передачи данных используется HTTPS.
Протестировать взаимодействия с API можно на [демо-сервере](https://vozovoz.xyz/dev/api/).

## Содержание

* [Быстрый старт](docs/quick.md)
    * [Расчёт стоимости заказа](docs/object/price.md)
    * [Оформление заказа](docs/object/order.md#set)
* [Параметры запросов](docs/params/index.md)
    * [Авторизация](docs/params/auth.md)
* [Объекты](docs/object/index.md)
    * [Локация](docs/object/location.md)
    * [Заказ](docs/object/order.md)
        * [Получить](docs/object/order.md#get)
        * [Оформить](docs/object/order.md#set)
    * [Прямой запрос](docs/object/directQuery.md)
    * [Расписание дат доставки](docs/object/schedule.md)
    * [Стоимость](docs/object/price.md)
    * [Терминал](docs/object/terminal.md)
    * [Упаковка](docs/object/wrapping.md)
* [Структуры](docs/structure/index.md)
    * [Груз](docs/structure/cargo.md)
        * [Габариты](docs/structure/cargo.md#dimension)
        * [Упаковка](docs/structure/cargo.md#wrapping)
    * [Контрагент](docs/structure/customer.md)
    * [Пункт доступа](docs/structure/gateway.md)
        * [Точка доступа](docs/structure/gateway.md#point)
        * [Услуги](docs/structure/service.md)

***

По всем вопросам обращайтесь на почту [api@vozovoz.ru](mailto:api@vozovoz.ru).