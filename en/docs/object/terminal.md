# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](index.md) > Terminal


> **Object code: `terminal`**

## Contents

* [Description](#description)
* [Getting data for one or more terminals](#get)
    * [Example](#get-example)
    * [Description](#get-struct)
    * [Response data](#get-response)
        * [Example of a response](#get-response-example)
        * [Description of a response structure](#get-response-description)


## <a name="description"/>Description

Represents an object used to obtain data about our transportation terminals.

>In this manual for easier understanding a word **_structures_** will be used
to name any structured data (mostly that contains other structures inside,
but also that contains simple data types, like string, number, boolean).
We name **full** necessary data object, that used in a request to our server
&mdash; ["parameters"](../params/index.md). Key to pass "parameters" into a request
called "params". More about that [here](../params/post.md). And we name **_nodes_**
certain structures with a defined key, that this structure is assigned to.


## <a name="get"/>Getting data. Action `get`


### <a name="get-get"/>Request data

> In this request every structure is **_optional_**, that is, it can be omitted.


#### <a name="get-example"/>Общий пример структуры

```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "terminal",
     "action": "get",
     "params": {
       "location": "Санкт-Петербург", // отобрать по локации, также может быть использован уникальный ID локации
       // или
       // "terminal": "12345678-1234-1234-1234-1234567890ab", // уникальный ID самого терминала
       "limit": 10, // ограничение количества записей в ответе
       "offset": 10 // смещение по количеству записей в ответе
     }
}))
```

#### <a name="get-struct"/>Корневая структура. Передаётся напрямую в узел `params`

| Структура     | Тип       | Описание |
| ---------     | ---       | -------- |
| `limit`       | integer   | Ограничение количества записей в ответе |
| `location`    | string    | Может быть как строкой запроса локации, так и внутренним уникальным ID локации |
| `offset`      | integer   | Смешение по количеству записей в ответе |
| `terminal`    | string    | Внутренний уникальный ID запрашиваемого терминала |

>Вы можете использовать:
>* узел `location` для получения терминалов по локации;
>* узел `terminal` для получения данных по терминалу;
>* только узлы `limit` и `offset` для перебора всех терминалов.


### <a name="get-response"/>Данные ответа

* [Пример ответа](#get-response-example)
* [Описание данных ответа](#get-response-description)

<a name="get-response-example"/>Пример ответа:

```javascript
{
  "response": {
    "data": [
      {
          "guid": "f6809ac0-9153-11e8-811d-00155d903d0c", // уникальный внутренний ID терминала
          "location_guid": "e90f1820-0128-11e5-80c7-00155d903d03", // уникальный внутренний ID локации
          "coordinates": { // координаты
              "latitude": "55.91741", // ширина
              "longitude": "37.56931" // долгота
          },
          "name": "МКАД 84-ый км, вл.3А, стр.3", // название\короткий адрес
          "address": "Москва, МКАД 84-й км, вл.3А, стр.3", // полный адрес
          "note": "ТПЗ \"Алтуфьево\", платный въезд-80руб., тел.склада:+7-985-464-45-73", // заметки
          "location_country": "RU", // страна
          "location_name": "Москва г" // город
      },
      {
          "guid": "a9c91404-5456-11e9-bb9f-00155df27330",
          "location_guid": "e90f1820-0128-11e5-80c7-00155d903d03",
          "coordinates": {
              "latitude": "55.712284",
              "longitude": "37.904683"
          },
          "name": "1-ый Красковский проезд 38 А стр.39",
          "address": "Москва, проезд Красковский 1-й, д. 38А стр. 39",
          "note": "тел.склада: +7-985-120-69-93; платный въезд - 80р. включается в общий счет",
          "location_country": "RU",
          "location_name": "Москва г"
      }
      // кол-во терминалов зависит от их количества по указанному запросу и узла `limit`
    ],
    "meta": { // meta-данные
      "limit": 10, // заданное ограничение
      "offset": 10, // заданное смещение
      "total": 285 // общее количество записей
    }
  }
}
```

<a name="get-response-description"/>Описание данных ответа:

| Название      | Тип       | Описание |
| --------      | ---       | -------- |
| `address`     | string    | Адрес терминала |
| `guid`        | string    | Уникальный внутренний ID терминала |
| `location_guid` | string  | УНикальный внутренний ID локации |
| `location_name` | string  | Название локации |
| `name`        | string    | Наименование терминала |
| `note`        | string    | Заметки по терминалу для клиентов |

***
[▲ Up](#up)