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


#### <a name="get-example"/>General example of the structure

```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "terminal",
     "action": "get",
     "params": {
       "location": "Санкт-Петербург", // filter by location, a unique location ID can also be used
       // or
       // "terminal": "12345678-1234-1234-1234-1234567890ab", // unique internal terminal ID
       "limit": 10, // limiting the number of entries in the response
       "offset": 10 // offset by the number of entries in the response
     }
}))
```

#### <a name="get-struct"/>Root structure. Passed directly into the `params` node

| Structure  | Type    | Description                                                             |
|------------|---------|-------------------------------------------------------------------------|
| `limit`    | integer | Limiting the number of entries in a response                            |
| `location` | string  | Can be either a location query string or an internal unique location ID |
| `offset`   | integer | Offset by number of records in response                                 |
| `terminal` | string  | Internal unique ID of the requested terminal                            |

>You can use the following:
>* `location` node for getting terminals by location;
>* `terminal` node to receive terminal data;
>* only `limit` and `offset` nodes to iterate over all terminals.


### <a name="get-response"/>Response data

* [Example of a response](#get-response-example)
* [Description of a response](#get-response-description)

<a name="get-response-example"/>Example of a response

```javascript
{
  "response": {
    "data": [
      {
          "guid": "f6809ac0-9153-11e8-811d-00155d903d0c", // unique internal terminal ID
          "location_guid": "e90f1820-0128-11e5-80c7-00155d903d03", // unique internal location ID
          "coordinates": { // coordinates
              "latitude": "55.91741",
              "longitude": "37.56931"
          },
          "name": "МКАД 84-ый км, вл.3А, стр.3", // name (short address)
          "address": "Москва, МКАД 84-й км, вл.3А, стр.3", // full address
          "note": "ТПЗ \"Алтуфьево\", платный въезд-80руб., тел.склада:+7-985-464-45-73", // additional info
          "location_country": "RU", // country
          "location_name": "Москва г" // location
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
      // the number of terminals depends on their number for the specified request and the `limit` node
    ],
    "meta": { // meta data
      "limit": 10, // specified limit
      "offset": 10, // specified offset
      "total": 285 // total count of the records returned
    }
  }
}
```

<a name="get-response-description"/>Description of a response:

| Name            | Type   | Description                                   |
|-----------------|--------|-----------------------------------------------|
| `address`       | string | Terminal address                              |
| `guid`          | string | Unique internal terminal ID                   |
| `location_guid` | string | Unique internal location ID                   |
| `location_name` | string | Location name                                 |
| `name`          | string | Terminal name (usually, it's a short address) |
| `note`          | string | Notes on the terminal for clients             |

***
[▲ Up](#up)