# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Request parameters](index.md) > Query string

Contents
----------

* [Query string](#get)
* [Available parameters](#list)
* [Examples](#example)

## <a name="get"/>Query string

With any API request you can send some parameters using query string of the URI,
like setting search conditions, authentication data, or any other information.
You can use these parameters after "?" character in the URI string.
You can add parameters, joining them together with "&" character.
Do not use spaces.

> **Pay attention!**
>
> If you in development state or testing, use `vozovoz.org`, instead of `vozovoz.ru`.

Example
```
https://vozovoz.ru/api/?param1=value1&param2=value2&param3=value3
```


## <a name="list"/>Available parameters

| Parameter          | Type    | Acceptable values           | Description                                                                                                                  |
|--------------------|---------|-----------------------------|------------------------------------------------------------------------------------------------------------------------------|
| **Required**   
| token              | string  | `yourToken`, `orOtherStuff` | Identifies the user of this API                                                                                              |
| **Optional** 
| debugMode          | boolean | `true`, `false`             | Returns debug information in this response, if any available, and this parameter is set to `true`                            |
| reportAll          | boolean | `true`, `false`             | Returns non-fatal errors and warnings in this response, if any occured, and this parameter is set to `true`                  |
| version            | string  | `2`, `2.0`, `2.V`           | Sets API version to use for this request. **At the moment actual version is 2.V** or 2 for short. Version 1.0 has no changes |

>**Notice, that values in Query string do not need quotes, unlike in Request body**

## <a name="example"/>Examples

For standard practice this URI below will be enough to use if you set [Request body](post.md) and [token](auth.md) right:
```
https://vozovoz.ru/api/?token=yourToken
```

Variations
```
https://vozovoz.ru/api/?token=orOtherStuff&version=2&reportAll=false
```
```
https://vozovoz.ru/api/?token=randomStringOfCharsJustForAnExample&debugMode=true
```

***
[▲ Up](#up)