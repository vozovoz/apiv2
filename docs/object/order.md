# <a name="up"/>Vozovoz API 2.0

[Главная страница](/README.md) > [Объекты](index.md) > Заказ


> **Код объекта: `order`**

## Содержание

* [Описание](#description)
* [Получение данных по одному или нескольким заказам](#get)
    * [Общий пример структуры](#get-example)
    * [Подробное описание структуры](#get-struct)
    * [Данные ответа](#get-response)
    * [Примеры](#get-example-full)
* [Отправка данных. Оформление заказа](#set)
    * [Общий пример структуры](#set-example)
    * [Подробное описание структуры](#set-struct)
    * [Данные ответа](#set-response)
    * [Примеры](#set-example-full)


## <a name="description"/>Описание

Представляет собой объект, использующийся для оформления заказа или для получения данных по уже оформленным заказам.

> В данной документации для упрощения понимания **_структурами_** называются все данные, которые содержат какие-либо данные
> (как другие структуры, так и простые типы данных: строка, число, логический тип).
> Под ["параметрами"](../params/index.md) мы понимаем необходимый **полный** массив данных, что передаётся нашему серверу
> для получения определённого ответа. Приведённые структуры указываются внутри [POST-параметра](../params/post.md) `params`.


## <a name="get"/>Получение данных. Действие `get`

### Данные запроса

> В данном запросе все структуры являются **_необязательными_**. Т.е. могут отсутствовать.

***

Быстрая навигация

* [Общий пример структуры](#get-example)
* [Корневая структура](#get-struct)
    * ["Фильтр заказа"](#get-filters)
        * ["Фильтр по датам"](#get-filters-dates)
            * ["Фильтр по датам создания"](#get-filters-dates-created)
        * ["Фильтр по локациям"](#get-location)
    * ["Состояние заказа"](#get-mode)

***

#### <a name="get-example"/>Общий пример структуры

```javascript
// полное оформление кода на javascript для использования в консоли см. в разделе "Быстрый старт"
xhttp.send(JSON.stringify(
{
  "object": "order",
  "action": "get",
  "params": {
    "filters": {
      "dates": { // фильтр по датам
        "created": { // дата оформления заказа
          "from": "2017-05-01", // начальный дата диапазона выборки
          "to": "2017-06-15" // конечная дата диапазона выборки
        }
      },
      "location": {
        "dispatch": "12345678-1234-1234-1234-123456789012", // фильтр по локации отправления
        "destination": "12345678-1234-1234-1234-123456789012" // фильтр по локации получения
      },
      "number": [ // может быть одной строкой или массивом любых из указанных ниже данных
        "12345678-1234-1234-1234-123456789012", // уникальный внутренний идентификатор заказа
        "700099999", // уникальный внутренний номер заказа
        "CW23423/dsf332233 (май 2017)" // пользовательский номер заказа, если Вы указывали его при оформлении
      ]
    },
    "limit": 100, // ограничение количества результатов выборки
    "offset": 0, // смещение по выборке
    "mode": { // подробнее см. субструктуру "Состояние заказа"
      "isCanceled": false, // Должен ли заказ быть отменён для попадания в выборку
      "isGiven": false, // Должен ли заказ быть выдан для попадания в выборку
      "isPaid": false, // Должен ли быть оплачен заказ для попадания в выборку
      "isTaken": true
    },
    "tab": "onward" // "onward" - В пути, "unpaid" - Неоплаченные, подробнее в структуре...
  }
}
```

***

#### <a name="get-struct"/>Корневая структура. Передаётся напрямую в узел `params`

| Структура     | Тип       | Описание |
| ---------     | ---       | -------- |
| `filters`     | object    | [Структура "Фильтр заказа"](#get-filters) |
| `limit`       | integer   | Число ограничивает количество заказов, возвращаемых за один запрос |
| `offset`      | integer   | Число указывает на количество заказов, которые нужно пропустить, прежде чем начать выборку |
| `mode`        | object    | [Структура "Состояние заказа"](#get-mode). Взаимоисключающий узел с узлом `tab` |
| `tab`         | string    | Строка, указывающая на предопределённый фильтр заказа. Взаимоисключающий узел с узлом `mode`, поскольку содержит преднастроенные параметры для узла `mode` |

>Преопределённые фильтры заказа (узел `tab`), имеющиеся на данный момент:
>* Значение `onward` - "В пути" для узла `tab` определяет следующие значения для узла `mode`:
>```
>{
>  "isCanceled": false,
>  "isGiven": false
>}
>```
>* Значение `unpaid` - "Неоплаченные" для узла `tab` определеяет следующие значения для узла `mode`:
>```
>{
>  "isCanceled": false,
>  "isPaid": false,
>  "isTaken": true
>}
>```

***

#### <a name="get-filters"/>Субструктура "Фильтр заказа". Узел `params.filters`

> Входит в [корневую структуру](#get-struct) данных запроса под узлом `filters`.

| Название      | Тип       | Описание |
| --------      | ---       | -------- |
| `dates`       | object    | Субструктура ["Фильтр по датам"](#get-filters-dates) |
| `customer`    | object    | Субструктура ["Фильтр по контрагентам"](#get-filters-customer) |
| `location`    | object    | Субструктура ["Фильтр по локациям"](#get-filters-location) |
| `number`      | array\|string | Строка или массив с номерами заказа (или уникальными id заказа, или пользовательскими номерами, который Вы указывали при создании). _В этой структуре данные можно перемешивать как угодно_ |

***

#### <a name="get-filters-dates"/>Субструктура "Фильтр по датам". Узел `params.filters.dates`

> Входит в субструктуру ["Фильтр заказа"](#get-filters) данных запроса под узлом `dates`.

На данный момент доступен только один узел фильтрации дат: `created`. Он отвечает за дату создания заказа.

| Название      | Тип       | Описание |
| --------      | ---       | -------- |
| `created`     | object    | Субструктура ["Фильтр по датам создания"](#get-filters-dates-created) |

***

#### <a name="get-filters-dates-created"/>Субструктура "Фильтр по датам создания". Узел `params.filters.dates.created`

> Входит в субструктуру ["Фильтр по датам"](#get-filters-dates) данных запроса под узлом `created`.

| Название      | Тип       | Описание |
| --------      | ---       | -------- |
| `from`        | string    | Строка формата `YYYY-MM-DD`, отвечающая за первую дату диапазона фильтрации по дате создания заказа и до сегодняшнего дня, или дня, указанного в узле `to` |
| `to`          | string    | Строка формата `YYYY-MM-DD`, отвечающая за последнюю дату диапазона фильтрации по дате создания заказа |

***

#### <a name="get-filters-location"/>Субструктура "Фильтр по локациям". Узел `params.filters.location`

> Входит в субструктуру ["Фильтр заказа"](#get-filters) данных запроса под узлом `location`.

| Название      | Тип       | Описание |
| --------      | ---       | -------- |
| `destination` | string    | Уникальный идентификатор локации. Вернёт только те заказы, где данная локация - локация **получения** |
| `dispatch`    | string    | Уникальный идентификатор локации. Вернёт только те заказы, где данная локация - локация **отправки** |

***

#### <a name="get-mode"/>Субструктура "Состояние заказа". Узел `params.mode`

> Входит в [корневую структуру](#get-struct) данных запроса под узлом `mode`.

Неуказание состояния подразумевает, что нас устраивают оба варианта состояния заказов, попадающих в результативную выборку.
Указание `false` настаивает на том, чтобы заказы, **имеющие** данное состояние, были исключены из выборки. Указание `true`
настаивает на том, чтобы заказы, **не имеющие** данное состояние, были исключены из выборки.

| Название      | Тип       | Описание |
| --------      | ---       | -------- |
| `isCanceled`  | boolean   | Должен ли заказ быть отменён для попадания в выборку |
| `isGiven`     | boolean   | Должен ли заказ быть выдан для попадания в выборку |
| `isPaid`      | boolean   | Должен ли быть оплачен заказ для попадания в выборку |
| `isTaken`     | boolean   | Должен ли заказ быть уже принятым на терминале для попадания в выборку |

***


### <a name="get-response"/>Данные ответа

* [Пример ответа](#get-response-example)
* [Описание данных ответа](#get-response-description)

<a name="get-response-example"/>Пример ответа:

```javascript
{
  "response": {
    "data": [ // массив заказов
      {
        "number": "700999999", // уникальный внутренний номер заказа
        "id": "12345678-1234-1234-1234-123456789012", // уникальный внутренний ID заказа
        "customId": "CW2343443//021394993 май 2017", // пользовательский ID, указанный при оформлении
        "status": "Заказ отменен", // информация по текущему статусу заказа
        "dates": {
          "create": "2016-11-21T17:10:24", // дата создания
          "taken": "", // дата приёма на терминале
          "shipped": "", // дата отправки груза на терминал получения
          "arrived": "", // дата прибытия груза на терминал получения
          "given": "" // дата выдачи груза клиенту
        },
        "basePrice": 1020, // базовая стоимость заказа (без скидок)
        "price": 1020, // итоговая стоимость заказа (с учётом скидок)
        "service": [ // массив услуг
          {
            "name": "Перевозка между городами", // название услуги
            "price": 190 // стоимость услуги (с учётом скидок)
          },
          {
            "name": "Забор груза от клиента",
            "price": 380
          },
          {
            "name": "Отвоз груза клиенту",
            "price": 450
          }
        ],
        "dimension": { // объект габаритов груза. См. струкутура "Груз"
          "max": {
            "height": 0.1, // макс. высота одного места
            "length": 0.1, // макс. длина одного места
            "weight": 0.1, // макс. вес одного места
            "width": 0.1 // макс. ширина одного места
          },
          "quantity": 1, // количество мест в заказе
          "volume": 0.1, // общий объём всего груза
          "weight": 0.9 // общий вес всего груза
        }
      },
      {
        "number": "700999999",
        "id": "12345678-1234-1234-1234-1234567890ab",
        "status": "Заказ отменен",
        "dates": {
          "created": "2017-03-16T16:49:36",
          "taken": "",
          "shipped": "",
          "arrived": "",
          "given": ""
        },
        "basePrice": 480,
        "price": 480,
        "service": [
          {
            "name": "Страхование груза без объявленной стоимости",
            "price": 35
          },
          {
            "name": "Платный въезд (отправитель)",
            "price": 50
          },
          {
            "name": "Платный въезд (получатель)",
            "price": 50
          },
          {
            "name": "Перевозка между городами",
            "price": 345
          }
        ],
        "dimension": {
          "max": {
            "height": 0.1,
            "length": 0.1,
            "weight": 0.1,
            "width": 0.1
          },
          "quantity": 1,
          "volume": 0.1,
          "weight": 0.9
        }
      }
    ],
    "meta": { // meta-данные
      "limit": 100, // ограничение количества заказов, выведенных на текуший запрос
      "offset": 0, // смещение по массиву заказов, выведенных на текущий запрос
      "total": 2 // всего заказов пользователя
    }
  }
}
```

<a name="get-response-description"/>Описание данных ответа:

| Название      | Тип       | Описание |
| --------      | ---       | -------- |
| `basePrice`   | integer   | Базовая стоимость в рублях (без учёта скидок) |
| `customId`    | string    | Пользовательский номер заказа, если был указан при оформлении |
| [`dates`](../structure/dates.md) | object | Объект, содержащий даты изменения этапов заказа. См. [структуру "Даты изменения этапов заказа"](../structure/dates.md) |
| [`dimension`](../structure/cargo.md#dimension) | object | Объект габаритов груза. См. [структуру "Габариты"](../structure/cargo.md#dimension) |
| `id`          | string    | Уникальный внутренний ID заказа |
| `number`      | integer   | Уникальный внутренний номер заказа |
| `price`       | integer   | Итоговая стоимость в рублях (с учётом скидок) |
| `service`<br/>- `name`</br>- `price` | array<br/>- string<br/>- integer | Массив услуг, с указанием наименования и стоимости (со скидкой)<br/>- Наименование услуги<br/>- Стоимость услуги со скидкой |
| `status`<br/>- `code`<br/>- `message` | object<br/>- string<br/>- string | Объект, содержащий текущий статус заказа<br/>- код текущего статуса<br/>- сообщение текущего статуса |


### <a name="get-example-full"/>Примеры

**Запрос**

```javascript
// полный вызов в консоли на JavaScript см. в разделе Быстрый старт
xhttp.send(JSON.stringify(
{
  "object": "order",
  "action": "set",
  "params": {
    "filters": {
      "number": "МойПользовательскийНомер"
    }
  }
}
```

**Ответ**

```javascript
{
  "response": {
    "data": [
      {
        "number": "700247224",
        "id": "40a18ac7-46d0-11e7-80f9-00155d189b14",
        "status": "Заказ оформлен",
        "dates": {
          "create": "2017-06-01T16:43:18",
          "taken": "",
          "shipped": "",
          "arrived": "",
          "given": ""
        },
        "basePrice": 2440,
        "price": 2440,
        "service": [
          {
            "name": "Страхование груза без объявленной стоимости",
            "price": 35
          },
          {
            "name": "Жесткий короб",
            "price": 800
          },
          {
            "name": "Упаковка в коробку (20х20х40)",
            "price": 140
          },
          {
            "name": "Перевозка между городами",
            "price": 495
          },
          {
            "name": "Забор груза от клиента",
            "price": 380
          },
          {
            "name": "Отвоз груза клиенту",
            "price": 590
          }
        ],
        "cargo": {
          "dimension": {
            "max": {
              "height": 0.1,
              "length": 0.1,
              "weight": 0.1,
              "width": 0.9
            },
            "quantity": 1,
            "volume": 0.1,
            "weight": 0.9
          },
          "wrapping": {
            "hardBoxVolume": 0.1,
            "box1": 2
          }
        },
        "customId": "МойПользовательскийНомер",
        "gateway": {
          "dispatch": {
            "point": {
              "location": {
                "id": "e90f19de-0128-11e5-80c7-00155d903d03",
                "name": "Санкт-Петербург",
                "type": "г"
              },
              "address": "Пивоварная ул. 11",
              "date": "2017-06-03",
              "time": {
                "start": "10:00",
                "end": "16:00"
              }
            },
            "customer": {
              "type": "individual",
              "name": "Name 1",
              "phone": [
                "79991112233"
              ],
              "email": ""
            }
          },
          "destination": {
            "point": {
              "location": {
                "id": "e90f1820-0128-11e5-80c7-00155d903d03",
                "name": "Москва",
                "type": "г"
              },
              "address": "Самоварная ул., 1",
              "date": "2017-06-07",
              "time": {
                "start": "10:00",
                "end": "16:00"
              }
            },
            "customer": {
              "type": "individual",
              "name": "Name 2",
              "phone": [
                "79998887766"
              ],
              "email": ""
            }
          }
        }
      }
    ],
    "meta": {
      "limit": 100,
      "offset": 0,
      "total": 1
    }
  }
}
```







## <a name="set"/>Получение данных. Действие `set`

### Данные запроса

* [Общий пример структуры](#set-example)
* [Корневая структура](#set-struct)

***

#### <a name="set-example"/>Общий пример структуры

```javascript
// полное оформление кода на javascript для использования в консоли см. в разделе "Быстрый старт"
xhttp.send(JSON.stringify(
{
  "object": "order",
  "action": "set",
  "params": {
    "cargo": {
      "category": "120ef1-4039-394903-1234-123456", // уникальный внутренний ID категории груза
      "correspondence": true, // перевозится ли корренспонденция
      "dimension": { // габариты груза
        "max": { // максимальные габариты одного места
          "height": 0.5, // макс. высота одного места
          "length": 0.5, // макс. длина одного места
          "weight": 5.3, // макс. вес одного места
          "width": 0.5 // макс. ширина одного места
        },
        "quantity": 2, // количество мест
        "volume": 0.9, // общий объём груза (всех мест)
        "weight": 5.3 // общий вес груза (всех мест)
      },
      "insurance": 1000, // заявленная стоимость для страховки (если требуется)
      "wrapping": { // упаковка
        "bag1": 1, // 1 шт. упаковки с кодом `bag1`
        "box1": 2, // 2 шт. упаковки с кодом `box1`
        "palletCollar": 0.5 // 0.5м3 упаковки с кодом `palletCollar`
      }
    },
    "customId": "MW0123456/ZX15", // пользовательский внешний ID. Может быть любимым. Необязателен
    "gateway": { // пункт доступа
      "dispatch": { // пункт отправки
        "point": { // точка доступа
          "location": "Санкт-Петербург", // локация отправки
          "address": "Луначарского ул., 7", // адрес отправки
          "date": "2017-05-01", // дата отправки
          "time": { // диапазон времени для забора груза
            "start": "14:00",
            "end": "18:00"
          }
        },
        "service": { // услуги
          "needLoading": { // погрузочные работы
            "floor": 21, // 21 этаж
            "hasLift": true // имеется грузовой лифт
          },
          "specificLoading": [ // особый вид транспорта
            "openMachine" // открытая машина
          ],
          "retrieveAD": { // возврат сопроводительных документов
            "point": { // точка доступа
              "location": "Выборг", // локации доставки СД
              "address": "Просвещения пр-т" // адрес доставки СД
            }
          }
        },
        "customer": { // контрагент (отпавитель)
          "email": "customer@ya.ru", // email
          "id": "ivan", // временный ID для операций запроса. Необязательный. По умолчанию `dispatch`
          "type": "individual", // физ. лицо
          "name": "Иванов Иван Иванович", // ФИО
          "phone": "79999999999" // телефон
        }
      },
      "destination": { // пункт получения
        "point": { // точка доступа
          "location": "Москва", // локация получения
          "terminal": "1c9871-e09d-df43423-1234-123456", // уникальный внутренний ID терминала
          "date": "2017-05-08" // дата получения
        },
        "service": { // услуги
          "needLoading": { // разгрузочные работы
            "used": false // не требуются
          },
          "specificLoading": [ // особый вид транспорта
            "tailLift"
          ]
        },
        "customer": { // контрагент (получатель)
          "type": "corporation", // юр. лицо
          "company": "АЛЬЯНС", // название юр. лица
          "name": "Ivanov Ilya", // ФИО контактного лица
          "phone": [ // список телефонов
            "79999999999",
            "79998888888"
          ],
          "kpp": "111111111", // КПП
          "inn": "222222222222" // ИНН
        }
      }
    },
    "payer": "ivan", // указывает плательщиком отправителя (по временному ID). Можно не указывать, отправитель - плательщик по умолчанию
    "promoCode": "promo" // промокод, если имеется
  }
}
```

#### <a name="get-struct"/>Корневая структура. Передаётся напрямую в узел `params`

| Структура         | Тип       | Описание |
| ---------         | ---       | -------- |
| **Обязательные**
| `cargo`           | object    | [Структура "Груз"](../structure/cargo.md) _(на другой странице)_ |
| `gateway`         | object    | [Структура "Пункт доступа"](../structure/gateway.md) _(на другой странице)_ |
| `payer`           | string\|object | Строка с временным ID, указанным (или не указанным) у контрагентов в структуре "Пункт доступа". По умолчанию имеет значение `dispatch`, т.е. отправитель - плательщик (если Вы хотите назначить плательщиком получателя, без ввода своего ID, укажите `destination`). **Также может быть третьим лицом**, тогда его содержимое должно соответствовать [структуре "Контрагент"](../structure/customer.md) _(на другой странице)_|
| **Необязательные**
| `customId`        | string    | Строка с Вашим ID для заказа. По ней впоследствии можно будет получить данные заказа. Необязательный параметр, каждый заказ в любом случае имеет уникальный внутренний номер и уникальный внутренний ID |
| `promoCode`       | string    | Промокод, если имеется. Необязательный параметр

### <a name="get-response"/>Данные ответа

* [Пример ответа](#set-response-example)
* [Описание данных ответа](#set-response-description)

<a name="set-response-example"/>Пример ответа:

```javascript
{
  "response": {
    "id": "3afda4a6-46cf-11e7-80f9-00155d189b14", // уникальный внутрениий ID заказа
    "number": "700247223", // уникальный внутренний номер заказа
    "basePrice": 2440, // базовая стоимость заказа (без скидок)
    "price": 2440, // итоговая стоимость заказа (со скидками)
    "service": [ // услуги
      {
        "name": "Перевозка между городами", // наименование услуги
        "price": 495 // стоимость услуги со скидкой
      },
      {
        "name": "Жесткий короб",
        "price": 800
      },
      {
        "name": "Упаковка в коробку (20х20х40)",
        "price": 140
      },
      {
        "name": "Страхование груза без объявленной стоимости",
        "price": 35
      },
      {
        "name": "Забор груза от клиента",
        "price": 380
      },
      {
        "name": "Отвоз груза клиенту",
        "price": 590
      }
    ]
  }
}
```

<a name="set-response-description"/>Описание данных ответа:

| Название      | Тип       | Описание |
| --------      | ---       | -------- |
| id            | string    | Уникальный внутренний ID заказа |
| number        | string    | Уникальный внутренний номер заказа |
| basePrice     | integer   | Базовая стоимость в рублях (без учёта скидок) |
| price         | integer   | Итоговая стоимость в рублях (с учётом скидок) |
| service<br/>- name</br>- price | array<br/>- string<br/>- integer | Массив услуг, с указанием наименования и стоимости (со скидкой)<br/>- Наименование услуги<br/>- Стоимость услуги со скидкой |

### <a name="set-example-full"/>Примеры

**Запрос**

```javascript
// полный вызов в консоли на JavaScript см. в разделе Быстрый старт
xhttp.send(JSON.stringify(
{
  "object": "order",
  "action": "set",
  "params": {
    "cargo": {
      "dimension": {
        "volume": 0.1,
        "weight": 0.9,
      },
      "wrapping": {
        "box1": 2,
        "hardBoxVolume": 0.5
      }
    },
    "gateway": {
      "dispatch": {
        "point": {
          "location": "Санкт-Петербург",
          "address": "Пивоварная ул. 11",
          "date": "2017-06-03",
          "time": {
            "start": "10:00",
            "end": "16:00"
          }
        },
        "customer": {
          "name": "Name 1",
          "phone": "79092407718"
        }
      },
      "destination": {
        "point": {
          "location": "Москва",
          "address": "собаки кусок, 1",
          "date": "2017-06-07",
          "time": {
            "start": "10:00",
            "end": "16:00"
          }
        },
        "customer": {
          "id": "name2",
          "name": "Name 2",
          "phone": "79092407700"
        }
      }
    },
    "payer": "name2"
  }
}));
```

**Ответ**

```javascript
{
  "response": {
    "id": "3afda4a6-46cf-11e7-80f9-00155d189b14", // уникальный внутрениий ID заказа
    "number": "700247223", // уникальный внутренний номер заказа
    "basePrice": 2440, // базовая стоимость заказа (без скидок)
    "price": 2440, // итоговая стоимость заказа (со скидками)
    "service": [ // услуги
      {
        "name": "Перевозка между городами", // наименование услуги
        "price": 495 // стоимость услуги со скидкой
      },
      {
        "name": "Жесткий короб",
        "price": 800
      },
      {
        "name": "Упаковка в коробку (20х20х40)",
        "price": 140
      },
      {
        "name": "Страхование груза без объявленной стоимости",
        "price": 35
      },
      {
        "name": "Забор груза от клиента",
        "price": 380
      },
      {
        "name": "Отвоз груза клиенту",
        "price": 590
      }
    ]
  }
}
```


***
[▲ Наверх](#up)