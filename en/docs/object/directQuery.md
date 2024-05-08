# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](index.md) > Direct query

> **Object code: `directQuery`**


## Contents

* [Example](#example)
* [Description](#description)
* Available methods:
    * [Available dates for shipping cargo](directQuery/getDepartureTimetable.md) `getDepartureTimetable`
    * [Available dates for receiving cargo](directQuery/getArrivalTimetable.md) `getArrivalTimetable`
    * [List of cargo categories](directQuery/getCargoTypes.md) `getCargoTypes`


## <a name="example"/>Examples

**Request**
```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "directQuery",
  "action": "get",
  "params": {
    "method": "test",
    "data": null // no need to pass it for this method, can be omitted
  }
}
```

**Response**
```json
{
  "response": {
    "currentdate": "2017-05-29T15:25:03Z"
  }
}
```

## <a name="description"/>Description
Represents an object that queries the database directly.


### Request data

| Structure | Type   | Description                                    |
|-----------|--------|------------------------------------------------|
| Required  |
| `method`  | string | Name of the method to call                     |
| Optional  |
| `data`    | object | Data required for the specified method to work |


### Response data

The response data varies depending on the method called.


***
[▲ Up](#up)