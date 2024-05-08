# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > List of the objects

## Contents

* [Description](#object)
* [List of the objects](#list)
* [List of the methods](#action)

## <a name="object"/>Objects

Represent a set of structures that returns certain data in response to your request depending on specified [method](#action) and [parameters](../params/post.md).

## <a name="list"/>List of the objects

| Object                                      | Object code   | Available methods | Description                                                                                                                                                                               |
|---------------------------------------------|---------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Cost](price.md)                            | `price`       | `get`             | Represents an object containing both the total cost and the cost for a specific service                                                                                                   |
| <nobr>[Delivery dates](schedule.md)</nobr>  | `schedule`    | `get`             | An object that returns the available shipping (dispatch) and delivery (destination) dates                                                                                                 |
| <nobr>[Direct query](directQuery.md)</nobr> | `directQuery` | `get`             | To support older methods, there is a direct query object that allows you to send a query directly to database                                                                             |
| [Location](location.md)                     | `location`    | `get`             | Represents both individual location objects and a list of them, and forms depending on the request parameters                                                                             |
| [Order](order.md)                           | `order`       | `get`, `set`      | Order object. The `get` action returns information on placed order, the `set` action is used to place a new order, or edit an existing one (in both cases it's called 'finalizing order') |
| [Terminal](terminal.md)                     | `terminal`    | `get`             | Represents both individual terminal objects and a list of them, and forms depending on the request parameters                                                                             |
| [Wrapping](wrapping.md)                     | `wrapping`    | `get`             | Represents both individual wrapping objects and a list of them, and forms depending on the request parameters                                                                             |
| Version                                     | `version`     | `get`             | API version. Returns a string representation of the current Vozovoz API version being used. Examples of this request can be found in [Quick start](../quick.md) section                   |


## <a name="action"/>List of the methods

| Method | Description                                                                           |
|--------|---------------------------------------------------------------------------------------|
| `get`  | Get data from server _(default method, i.e. can be omitted in the request)_           |
| `set`  | Send data to server _(to add or update depending on [parameters](../params/post.md))_ |

***
[▲ Up](#up)