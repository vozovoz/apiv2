# <a name="up"/>Vozovoz API 2.0

[Главная страница](/README.md) > [Параметры запроса](index.md) > POST-параметры

## Содержание

* [POST-параметры](#post)
* [Список доступных POST-параметров](#list)
* [Примеры](#example)


## <a name="post"/>POST-параметры

Метод запроса POST предназначен для запроса, при котором веб-сервер принимает данные, заключённые в тело сообщения, для хранения. Он часто используется для загрузки файла или представления заполненной веб-формы.

В зависимости от среды разработки POST-параметры передаются по-разному. Вот пара примеров.

PHP:
```php
$context = stream_context_create([
  'http' => [
    'method' => 'POST',
    'header' => 'Content-type: application/json',
    'content' => json_encode([
      'object' => 'version',
      'action' => 'get',
    ]),
  ]
]);

/* запрос на тестовый сервер */
echo file_get_contents('https://qa.vozovoz.ru/api/?token=yourToken'/* не забудьте указать Ваш токен */, false, $context);
/* запрос на рабочий сервер */
// echo file_get_contents('https://vozovoz.ru/api/?token=yourToken'/* не забудьте указать Ваш токен */, false, $context);
```

JavaScript:
```javascript
var xhttp = new XMLHttpRequest();
// запрос на тестовый сервер
xhttp.open("POST", "//qa.vozovoz.ru/api/?token=yourToken"/*вместо yourToken должен быть указан Ваш идентификационный токен-ключ*/, false);
// xhttp.open("POST", "//vozovoz.ru/api/?token=yourToken", false); // для рабочего сервера
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.send(JSON.stringify({
// Тело запроса, например:
  "object": "version",
  "action": "get"
}));
// вывести текст тут же, в консоли
xhttp.responseText;
// или разместить его прямо на странице
//document.body.innerHTML = xhttp.responseText;
```


## <a name="list"/>Список доступных POST-параметров

| Параметр | Тип | Значения | Описание |
| -------- | --- | -------- | -------- |
| **Обязательные**
| `object` | string | `version`, `location` и т.п. | Указывает на объект, с которым будет производится действие. Полный список объектов [тут](../object/index.md) |
| **Необязательные**
| `action` | string | `get`, `put` | Указывает на действие, производимое с объектом. По умолчанию `get` |
| `params` | object | `{"param1": "value1", "param2": "value2"}` | Передает необходимые параметры уточнения запроса. Представляет собой объект (ассоциативный массив). Может отсутствовать. Подробнее о структурах данных [здесь](../structure/index.md) |

## <a name="example"/>Примеры

```javascript
xhttp.send(JSON.stringify({
  "object": "location",
  "params": {
    "search": "санкт",
    "limit": 25
  }
}));
```

```php
$context = stream_context_create([
  'http' => [
    'method' => 'POST',
    'header' => 'Content-type: application/json',
    'content' => json_encode([
      'object' => 'terminal',
      'action' => 'get',
      'params' => [
	    'guid' => '99999894-3673-11e5-80cd-00155d903d03',
      ],
    ]),
  ]
]);
```

***
[▲ Наверх](#up)