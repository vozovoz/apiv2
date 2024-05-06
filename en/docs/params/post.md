# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Request parameters](index.md) > Request body

## Contents

* [Request body](#post)
* [Available parameters](#list)
* [Examples](#example)


## <a name="post"/>Request body

Depending on the development environment body of `POST` request sets in different ways. Here some examples. 

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

/* demo server */
echo file_get_contents('https://vozovoz.org/api/?token=yourToken'/* don't forget to use Your token */, false, $context);
/* production server */
// echo file_get_contents('https://vozovoz.ru/api/?token=yourToken'/* don't forget to use Your token */, false, $context);
```

JavaScript:
```javascript
var xhttp = new XMLHttpRequest();
// demo server
xhttp.open("POST", "//vozovoz.org/api/?token=yourToken"/*replace `yourToken` with your auth token*/, false);
// xhttp.open("POST", "//vozovoz.ru/api/?token=yourToken", false); // production server
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.send(JSON.stringify({
// request body, for example:
  "object": "version",
  "action": "get"
}));
// print text in the console
xhttp.responseText;
// or on the webpage
//document.body.innerHTML = xhttp.responseText;
```


## <a name="list"/>Available parameters

| Parameter          | Type      | Acceptable values                          | Description                                                                                                                                                          |
|--------------------|-----------|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Required**   
| `object`           | string    | `version`, `location`, etc.                | Sets the object for this request. Full list of available objects [here](../object/index.md)                                                                          |
| **Optional** 
| `action`           | string    | `get`, `put`                               | Sets the action for this request. This action will be applied to the object. Default action is `get`                                                                 |
| `params`           | structure | `{"param1": "value1", "param2": "value2"}` | Sets parameters for a proper setup of the action. Has the form of a standard JSON object (key-pair). Full list of available structures [here](../structure/index.md) |

## <a name="example"/>Examples

This method searches through our locations that have 'санкт' in the name, and returns the list of found locations (not more than 25 elements). NOTE: `action` not defined, so it sets to `get`

JavaScript:
```javascript
xhttp.send(JSON.stringify({
  "object": "location",
  "params": {
    "search": "санкт",
    "limit": 25
  }
}));
```

This method returns information about our terminal with presented GUID.

PHP:
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
[▲ Up](#up)