# <a name="up"/>Vozovoz API 2.0

[Главная страница](/README.md) > [Объекты](index.md) > Прямой запрос

> **Код объекта: `directQuery`**


## Содержание

* [Примеры](#example)
* [Описание](#description)
* Доступные методы
    * [Доступные даты отправки груза](directQuery/getDepartureTimetable.md) `getDepartureTimetable`
    * [Доступные даты получения груза](directQuery/getArrivalTimetable.md) `getArrivalTimetable`
    * [Список категорий груза](directQuery/getCargoTypes.md) `getCargoTypes`


## <a name="example"/>Примеры

**Запрос**
```javascript
// полное оформление кода на javascript для использования в консоли см. в разделе "Быстрый старт"
xhttp.send(JSON.stringify(
{
  "object": "directQuery",
  "action": "get",
  "params": {
    "method": "test",
    "data": null // для данного метода не передаётся, в этом случае можно вообще не указывать
  }
}
```

**Ответ**
```json
{
  "response": {
    "currentdate": "2017-05-29T15:25:03Z"
  }
}
```

## <a name="description"/>Описание
Представляет собой объект, осуществляющий запрос к базе данных напрямую.


### Данные запроса

| Структура     | Тип | Описание |
| ---------     | --- | -------- |
| Обязательные
| `method`      | string | Название метода, которое необходимо выполнить |
| Необязательные
| `data`        | object | Данные, необходимые для работы вызываемого метода |


### Данные ответа

Данные ответа разнятся в зависимости от вызванного метода.


***
[▲ Наверх](#up)