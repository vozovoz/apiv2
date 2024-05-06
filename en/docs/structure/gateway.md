# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Request data structures](index.md) > Gateway

## Contents

* [Base structure description](#struct)
    * [`point` structure (Access point)](#point)
    * [`service` structure](service.md) _(on another page)_
    * [`customer` structure](customer.md) _(on another page)_
* [Examples](#example)


## <a name="struct"/>Base structure description

`gateway` structure always consists of two nodes, that have similar structure.
They are `dispatch` (describes 'left part', where a shipment goes from, shipper information, etc.)
and `destination` (describes 'right part', where to deliver, receiver information, etc.).
If there are no special notes, consider that every described structure below can be in any of the these nodes.

>In this manual for easier understanding a word **_structures_** will be used
to name any structured data (mostly that contains other structures inside,
but also that contains simple data types, like string, number, boolean).
We name **full** necessary data object, that used in a request to our server
&mdash; ["parameters"](../params/index.md). Key to pass "parameters" into a request
called "params". More about that [here](../params/post.md). And we name **_nodes_**
certain structures with a defined key, that this structure is assigned to.


### <a name="point"/>`point` structure (Access point)

| Name              | Type          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|-------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Required          |
| `location`        | string        | Unique inner identificator of a location, or a query string to search for                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `address`         | string\|array | Particular address for pickup/delivery. Mutually exclusive to `terminal`. Can be array of two strings: `["first address to get a cargo or documents", "second address for the same"]`)                                                                                                                                                                                                                                                                                                                                                                                         |
| `driverComment`   | string        | Commentary for a pickup/delivery driver. Ignored, if `terminal` is set                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `driverDataEmail` | string        | E-mail address where information about pickup/delivery driver will be sent to. Ignored, if `terminal` is set. <small>We draw your attention to the need to strictly maintain the confidentiality of the information you receive (information about passport data and other data of the representative of the shipping agent - the driver of the vehicle) and to ensure access to such information only to those persons whose job responsibilities include working with such information. By entering your email address, you automatically agree to the terms listed.</small> |
| `terminal`        | string        | Unique inner identificator of a terminal, or set value to `"default"`, which replaces value with a default terminal ID for a specified location                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Optional          |   
| `date`            | string        | Date in `YYYY-MM-DD` format that defines date of pickup/shipping/delivery                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `time`            | object        | Object, that defines time interval for pickup or delivery date                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `time.start`      | string        | Start of `time` diapason in `HH:mm` format                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `time.end`        | string        | End of `time` diapason in `HH:mm` format                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `time.fix`        | boolean       | Set to `true`, if you need "fixed time" service (`false` by default)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

> The `point` node, where `address` and `terminal` nodes not defined or empty, is considered as `address` node. It works for **cost calculation** request, but **order finalization** returns error.


**Example**
```javascript
{
  "location": "Санкт-Петербург", // location query string
  "address": "Северный пр., 18", // address string, can be replaced with "terminal": "default"
  "date": "2017-05-26", // date of pickup/delivery, depends on the node ("dispatch" или "destination" respectively)
  "time": { // time of pickup/delivery
    "start": "14:00", // start of time period
    "end": "15:00", // end of time period
    "fix": true // fixed time service
  }
}
```


***
[▲ Up](#up)