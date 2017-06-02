# <a name="up"/>Vozovoz API 2.0

[Главная страница](/README.md) > Быстрый старт

## Быстрый старт

Все запросы и ответы Vozovoz API передаются в формате JSON, передаваемого в [POST-параметрах](params/post.md).
Для использования Vozovoz API Вам необходимо в каждом запросе в [GET-параметрах](params/get.md) указывать свой идентификационный токен.

Пример кода на JavaScript, который Вы можете в любой момент ввести в консоли разработчика:

```javascript
var xhttp = new XMLHttpRequest();
xhttp.open("POST", "//vozovoz.ru/api/?token=yourToken"/*вместо yourToken должен быть указан Ваш идентификационный токен-ключ*/, false);
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

***
[▲ Наверх](#up)