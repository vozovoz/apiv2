# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](../index.md) > [Direct query](../directQuery.md) > Schedule of dates for receiving cargo

> **Object code: `directQuery`**<br/>
> **Method code: `getArrivalTimetable`**


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
    "method": "getArrivalTimetable",
    "data": {
      "from": { // shipping point (dispatch)
        "locationId": "f2a30387-0124-11e5-80c7-00155d903d03", // unique internal location ID (if dispatch is address)
        // or
        // "terminalId": "99999894-3673-11e5-80cd-00155d903d03", // if dispatch is terminal
        "dates": { // this `dates` object required, it dispatch is address
          "to": "2017-06-06T18:00:00Z" // shipping datetime lower bound
        }
      },
      "to": { // delivery point (destination)
        "locationId": "e90f1820-0128-11e5-80c7-00155d903d03" // unique internal location ID (if destination is address)
        // or
        // "terminalId": "30b8e091-3c6b-11e6-80e9-00155d903d0a" // if destination is terminal
      }
    }
  }
}));
```

<a name="response-example"/>**Response**

```javascript
{
  /* if it's `address-to-address` */
  "response": {
    "locationId": "e90f1820-0128-11e5-80c7-00155d903d03", // delivery to address (location ID)
    "dates": [
      {
        "from": "2017-06-08T14:00:00Z", // delivery datetime upper bound of diapason (start of the period)
        "to": "2017-06-08T19:00:00Z", // delivery datetime lower bound of diapason (end of the period)
        "courierArrivalTimeMinInterval": 5 // minimum range interval (in hours)
      },
      {
        "from": "2017-06-09T09:00:00Z",
        "to": "2017-06-09T19:00:00Z",
        "courierArrivalTimeMinInterval": 5
      }
    ]
  }
  /* if it's `address-to-terminal` */
  "response": {
    "terminalId": "30b8e091-3c6b-11e6-80e9-00155d903d0a", // delivery to terminal (terminal ID)
    "dates": [
      {
        "from": "2017-06-08T14:00:00Z", // delivery datetime (start of the period)
        "to": "2017-06-08T23:59:00Z" // lower bound (depending on working hours of a terminal), though "23:59" means it's 24 hours open
      },
      {
        "from": "2017-06-09T00:01:00Z",
        "to": "2017-06-09T23:59:00Z"
      },
      {
        "from": "2017-06-10T00:01:00Z",
        "to": "2017-06-10T23:59:00Z"
      }
    ]
  }
  /* it it's `terminal-to-terminal` */
  "response": {
    "terminalId": "30b8e091-3c6b-11e6-80e9-00155d903d0a",  // delivery to terminal (terminal ID)
    "dates": [
      {
        "from": "2017-06-07T14:00:00Z", // delivery datetime (start of the period)
        "to": "2017-06-07T23:59:00Z" // lower bound (depending on working hours of a terminal), though "23:59" means it's 24 hours open
      },
      {
        "from": "2017-06-08T00:01:00Z",
        "to": "2017-06-08T23:59:00Z"
      },
      {
        "from": "2017-06-09T00:01:00Z",
        "to": "2017-06-09T23:59:00Z"
      }
    ]
  }
  /* if it's `terminal-to-address` */
  "response": {
    "locationId": "e90f1820-0128-11e5-80c7-00155d903d03", // delivery is to address (location ID)
    "dates": [
      {
        "from": "2017-06-07T14:00:00Z", // delivery datetime upper bound of diapason (start of the period)
        "to": "2017-06-07T19:00:00Z", // delivery datetime lower bound of diapason (end of the period)
        "courierArrivalTimeMinInterval": 5 // minimum range interval (in hours)
      },
      {
        "from": "2017-06-08T09:00:00Z",
        "to": "2017-06-08T19:00:00Z",
        "courierArrivalTimeMinInterval": 5
      },
      {
        "from": "2017-06-09T09:00:00Z",
        "to": "2017-06-09T19:00:00Z",
        "courierArrivalTimeMinInterval": 5
      }
    ]
  }
}
```


## <a name="description"/>Description
The [direct query](../directQuery.md) object using `getArrivalTimetable` method returns the schedule of available delivery dates.


### Request data

| Structure  | Type   | Description                                                     |
|------------|--------|-----------------------------------------------------------------|
| Required   |   
| `method`   | string | The name of the method to call. Should be `getArrivalTimetable` |
| Optional   |
| `data`     | object | Data required for the called method to operate. See below       |

>The `data` node must contain:
>* `from` (object) - dispatch point
>* `to` (object) - destination point
>
>Each of these nodes may contain:
>* `locationId` (string) - unique internal location ID<br/>or
>* `terminalId` (string) - unique internal terminal ID
>
>And also `from` node may contain:
>* `dates` (object) with `to` node - lower limit of a shipping time range. Mandatory if dispatch is address


### Response data

>The response data is described as comments in [example above](#response-example).


***
[▲ Up](#up)