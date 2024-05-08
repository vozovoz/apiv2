# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](../index.md) > [Direct query](../directQuery.md) > Schedule of dates for shipping cargo

> **Object code: `directQuery`**<br/>
> **Method code: `getDepartureTimetable`**


## Contents

* [Example](#example)
* [Description](#description)

## <a name="example"/>Example

**Request**
```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "directQuery",
  "action": "get",
  "params": {
    "method": "getDepartureTimetable",
    "data": {
      "locationId": "f2a30387-0124-11e5-80c7-00155d903d03" // unique internal location ID (if address)
      // or
      // "terminalId": "99999894-3673-11e5-80cd-00155d903d03" // if terminal
    }
  }
}));
```

<a name="response-example"/>**Response**

```javascript
{
  /* if dispatch from address */
  "response": {
    "locationId": "f2a30387-0124-11e5-80c7-00155d903d03",
    "dates": [
      {
        "from": "2017-06-02T10:30:00Z", // start of the diapason (pay attention to time)
        "to": "2017-06-02T18:00:00Z", // end of the diapason
        "courierArrivalTimeMinInterval": 3 // minimum time range (in hours)
      },
      {
        "from": "2017-06-03T10:00:00Z",
        "to": "2017-06-03T14:00:00Z",
        "courierArrivalTimeMinInterval": 3
      },
      {
        "from": "2017-06-05T10:00:00Z",
        "to": "2017-06-05T18:00:00Z",
        "courierArrivalTimeMinInterval": 3
      },
      {
        "from": "2017-06-06T10:00:00Z",
        "to": "2017-06-06T18:00:00Z",
        "courierArrivalTimeMinInterval": 3
      },
      // etc.
    ]
  }
  /* if dispatch from terminal */
  "response": {
    "terminalId": "99999894-3673-11e5-80cd-00155d903d03",
    "dates": [
      {
        "from": "2017-06-02T10:30:00Z", // start of the diapason (pay attention to time)
        "to": "2017-06-02T19:00:00Z" // end of the diapason (for terminal it means end of the working hours)
      },
      {
        "from": "2017-06-05T09:00:00Z", // for terminal this node (except for the first date) usually means start of the working hours
        "to": "2017-06-05T19:00:00Z"
      },
      {
        "from": "2017-06-06T09:00:00Z",
        "to": "2017-06-06T19:00:00Z"
      },
      {
        "from": "2017-06-07T09:00:00Z",
        "to": "2017-06-07T19:00:00Z"
      },
      // etc.
    ]
  }
}
```


## <a name="description"/>Description

The [direct query](../directQuery.md) object using `getDepartureTimetable` method returns the schedule of available shipping dates.

### Request data

| Structure | Type   | Description                                                       |
|-----------|--------|-------------------------------------------------------------------|
| Required  |   
| `method`  | string | The name of the method to call. Should be `getDepartureTimetable` |
| Optional  | 
| `data`    | object | Data required for the called method to operate. See below         |

>The `data` node can contain:
>* `locationId` (string) - unique internal location ID<br/>or
>* `terminalId` (string) - unique internal terminal ID


### Response data

>The response data is described as comments in [example above](#response-example).


***
[▲ Up](#up)