# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](index.md) > Location

> **Object code: `location`**


## Contents

* [Examples](#example)
* [Description](#description)


## <a name="example"/>Examples

* [Example of the request with offset and limit](#limit-offset)
* [Example of requesting information for a specific location](#guid)
* [Example of requesting a list of locations by location name](#search)

### <a name="limit-offset"/>Example of the request with offset and limit

**Example of the request in JavaScript**
```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "location",
  "action": "get",
  "params": {
    "offset": 12,
    "limit": 2
  }
}
```

**Example of the response**
```javascript
{
  "response": {
    "data": [
      {
        "guid": "c3b30743-0125-11e5-80c7-00155d903d03", // unique internal location ID
        "name": "Волгоград", // name of the location
        "type": "г", // type of the location
        "region_str": "Волгоградская обл", // regional information
        "default_terminal": { // default terminal
          "guid": "09c9e60a-c8c7-11e4-80bf-00155d903d03", // unique internal terminal ID
          "name": "Домостроителей ул. 13" // address(name) of the terminal
        }
      },
      {
        "guid": "c9ace063-0125-11e5-80c7-00155d903d03",
        "name": "Вологда",
        "type": "г",
        "region_str": "Вологодская обл",
        "default_terminal": {
          "guid": "747266e9-9a8c-11e6-80f4-00155d903d0c",
          "name": "Пошехонское ш.18"
        }
      }
    ],
    "meta": { // meta data
      "limit": 2, // specified limit
      "offset": 12, // specified offset
      "total": 33165 // total count of the records for this request
    }
  }
}
```

### <a name="guid"/>Example of requesting information for a specific location

**Example of the request in JavaScript**
```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "location",
  "action": "get",
  "params": {
    "id": "e90f19de-0128-11e5-80c7-00155d903d03" // unique internal location ID
    // you can also search by settlements name and region, see example below
  }
}
```

**Example of the response**
```javascript
{
  "response": {
    "data": [
      {
        "guid": "e90f19de-0128-11e5-80c7-00155d903d03", // unique internal location ID
        "name": "Санкт-Петербург", // name of the location
        "type": "г", // type of the location
        "region_str": "Город федерального значения", // regional information
        "default_terminal": { // default terminal
          "guid": "0482c0d9-99b8-11e5-80d3-00155d903d03", // unique internal terminal ID
          "name": "Народная ул., у дома 65 (Союзпечать 2406)" // address(name) of the terminal
        }
      }
    ],
    "meta": { // meta data
      "limit": 100, // specified limit
      "offset": 0, // specified offset
      "total": 33165 // total count of the records for this request
    }
  }
}
```


### <a name="search"/>Example of requesting a list of locations by location name

**Example of the request in JavaScript**
```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "location",
  "action": "get",
  "params": {
    "search": "Киров, Калужская область", // can be specified like this
    "offset": 0, // optional, just for example
    "limit": 3 // optional, just for example
  }
}
```

**Example of the response**
```javascript
{
  // results are sorted by relevance
  "response": {
    "data": [
      {
        "guid": "3b22856a-0126-11e5-80c7-00155d903d03", // unique internal location ID
        "name": "Киров", // name of the location
        "type": "г", // type of the location
        "region_str": "Калужская обл, Кировский р-н" // regional information
      },
      {
        "guid": "b1633010-0126-11e5-80c7-00155d903d03",
        "name": "Кировск",
        "type": "г",
        "region_str": "Кировский р-н, Ленинградская обл"
      },
      {
        "guid": "f5006d71-0128-11e5-80c7-00155d903d03",
        "name": "Кировское",
        "type": "пгт",
        "region_str": "Крым Респ, Кировский р-н"
      }
    ]
  },
  "meta": {
    "limit": 3,
    "offset": 0,
    "total": 33165
  }
}
```

## <a name="description"/>Description

This is any residential area (city, village, town, etc.). You can request either an individual location or a list.


### Request data

> All request data is optional, i.e. can be omitted. If nothing is passed,
the result will be a list of the first 100 locations, sorted by priority index and then by name.

| Structure      | Type    | Description                                                                                                                                                                                                            |
|----------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `id`           | string  | A string with a unique internal location ID                                                                                                                                                                            |
| `limit`        | integer | Limit the number of results returned                                                                                                                                                                                   |
| `offset`       | integer | Offset the number of results returned                                                                                                                                                                                  |
| `search`       | string  | A string containing the location query. The node is mutually exclusive with the `id` node                                                                                                                              |
| `hasTerminals` | boolean | Specifies an additional filter for the selection. If `true`, only locations that have terminal(s) will be returned in the response. If `false`, only locations that does not. If omitted, this filter won't be applied |


### Response data

| Name                    | Type   | Description                                                               |
|-------------------------|--------|---------------------------------------------------------------------------|
| `guid`                  | string | A string with a unique internal location ID                               |
| `name`                  | string | Name of the location                                                      |
| `region_str`            | string | Information on the location's region (region, district, city, etc.)       |
| `type`                  | string | Abbreviated type of location                                              |
| `default_terminal`      | object | If the location has terminal(s), contains data about the default terminal |
| `default_terminal.guid` | string | ~ A string with a unique internal terminal ID                             |
| `default_terminal.name` | string | ~ Short address of the terminal                                           |


***
[▲ Up](#up)