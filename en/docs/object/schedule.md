# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](index.md) > Delivery dates


> **Object code: `schedule`**

## Contents

* [Description](#description)
* [Getting data. Action `get`](#get)
    * [Example of the structure](#get-example)
    * [Description of the structure](#get-struct)
    * [Response data](#get-response)
        * [Example of an "address-to-address" response](#get-response-example-addr-addr)
        * [Example fo a "terminal-to-terminal" response](#get-response-example-term-term)


## <a name="description"/>Description

Represents an object used to obtain data on available delivery dates.

>In this manual for easier understanding a word **_structures_** will be used
to name any structured data (mostly that contains other structures inside,
but also that contains simple data types, like string, number, boolean).
We name **full** necessary data object, that used in a request to our server
&mdash; ["parameters"](../params/index.md). Key to pass "parameters" into a request
called "params". More about that [here](../params/post.md). And we name **_nodes_**
certain structures with a defined key, that this structure is assigned to.


## <a name="get"/>Getting data. Action `get`


### <a name="get-get"/>Request data

>The `date` node in the `dispatch` object is an optional parameter,
but can speed up the request processing by several times.
Please also note that sending unique location\terminal IDs in the request also reduces time to get the response.

#### <a name="get-example"/>Example of the structure

```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "schedule",
     "action": "get",
     "params": {

       // pickup/shipping address (in free form) or unique internal location ID
       "dispatch": {"address": "Санкт-Петербург"},
       // or unique internal terminal ID
       // "dispatch": {"terminal": "12345678-1234-1234-1234-1234567890ab"},

       // delivery address (in free form) or unique internal location ID
       "destination": {"address" : "мск, ленинский"}
       // or delivery terminal (and yes, the terminal can also be specified in plain text)
       // "destination": {"terminal": "мск, МКАД 84-ый км"}

     }
}))
```

#### <a name="get-struct"/>Root structure. Passed directly into the `params` node

| Structure              | Type   | Description                                                                                                            |
|------------------------|--------|------------------------------------------------------------------------------------------------------------------------|
| `destination`          | object | Object containing delivery data                                                                                        |
| `destination.address`  | string | Free form delivery address query string or unique internal location ID. Mutually exclusive with `destination.terminal` |
| `destination.terminal` | string | Free form delivery terminal query string or unique internal terminal ID. Mutually exclusive with `destination.address` |
| `dispatch`             | object | Object containing shipping data                                                                                        |
| `dispatch.address`     | string | Free form pickup address query string or unique internal location ID. Mutually exclusive with `dispatch.terminal`      |
| `dispatch.date`        | string | Pickup/shipping date in `YYYY-MM-DD` format (for example, `2020-03-01` - 1st March of 2020). Optional parameter        |
| `dispatch.terminal`    | string | Free form shipping terminal query string or unique internal terminal ID. Mutually exclusive with `dispatch.address`    |

>You can use the following:
>* node `dispatch.date` to get the schedule for the selected date;
>* do not specify the `dispatch.date` parameter to obtain the current schedule for the next 15 days.


### <a name="get-response"/>Response data

* [Example of an "address-to-address" response](#get-response-example-addr-addr)
* [Example fo a "terminal-to-terminal" response](#get-response-example-term-term)

> NOTICE! You can use all combinations (not just those given in the example):
> - address-address
> - address-terminal
> - terminal-address
> - terminal-terminal

<a name="get-response-example-addr-addr"/>Example of an "address-to-address" response

```javascript
{
  "response": {
    "2019-11-28": { // a pickup/shipping date
      // standard cargo pickup interval for a given location (in hours)
      "hourInterval": 5, // that is, without "Fixed time" mode
      "period": { // available cargo pickup period for a given location
        "start": "07:00",
        "end": "23:30"
      },
      // available delivery dates for a given shipping date (in this case "2019-11-28")
      "destination": {
        "18:00": { // if the cargo was picked up before "18:00"
          "2019-12-04": {
            "hourInterval": 8, // standard cargo pickup interval for a given location (in hours)
            "period": { // available cargo delivery period for a given location
              "start": "10:00",
              "end": "18:00"
            }
          },
          "2019-12-06": {
            "hourInterval": 8,
            "period": {
              "start": "10:00",
              "end": "18:00"
            }
          }
        },
        "23:30": { // if the cargo was picked up before "23:30"
          "2019-12-06": {
            "hourInterval": 8,
            "period": {
              "start": "10:00",
              "end": "18:00"
            }
          },
          "2019-12-11": {
            "hourInterval": 8,
            "period": {
              "start": "10:00",
              "end": "18:00"
            }
          }
        }
      }
    },
    "2019-11-29": { // another pickup/shipping date
      //... another data on this date but in the same format as above
    }
  }
}
```

<a name="get-response-example-term-term"/>Example fo a "terminal-to-terminal" response

```javascript
{
  "response": {
    "2019-11-28": { // a pickup/shipping date
      // available delivery dates for a given shipping date (in this case "2019-11-28")
      "destination": {
        "18:00": { // if the cargo was delivered to the terminal on November 28, 2019 before “18:00”
          "2019-11-29": "14:00", // the cargo will be available on November 29, 2019 after "14:00"
          "2019-11-30": "anytime", // anytime (it means terminal works 24 hours)
          "2019-12-01": "anytime", 
          "2019-12-02": "anytime"  
        },
        "22:00": { // if the cargo was delivered to the terminal on November 28, 2019 before “22:00” (but after "18:00")
          "2019-11-30": "16:00", // the cargo will be available on November 30, 2019 after "16:00"
          "2019-12-01": "anytime", // anytime (it means terminal works 24 hours)
          "2019-12-02": "anytime", 
          "2019-12-03": "anytime"  
        }
      }
    },
    "2019-11-29": { // another pickup/shipping date
      //... another data on this date but in the same format as above
    }
  }
}
```

***
[▲ Up](#up)