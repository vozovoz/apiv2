# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](index.md) > Order


> **Object code: `order`**

## Contents

* [Description](#description)
* [Getting data for one or more orders](#get)
    * [Example of the structure](#get-example)
    * [Description of the structure](#get-struct)
    * [Response data](#get-response)
    * [Examples](#get-example-full)
* [Sending data. Finalizing an order](#set)
    * [Example of the structure](#set-example)
    * [Description of the structure](#set-struct)
    * [Response data](#set-response)
    * [Examples](#set-example-full)
* [Canceling an order](#cancel)


## <a name="description"/>Description

Represents an object used to place (finalize) an order or to obtain data on already placed order(s).

>In this manual for easier understanding a word **_structures_** will be used
to name any structured data (mostly that contains other structures inside,
but also that contains simple data types, like string, number, boolean).
We name **full** necessary data object, that used in a request to our server
&mdash; ["parameters"](../params/index.md). Key to pass "parameters" into a request
called "params". More about that [here](../params/post.md). And we name **_nodes_**
certain structures with a defined key, that this structure is assigned to.


## <a name="get"/>Getting data. Action `get`

### Request data

> In this request every structure is **_optional_**, that is, it can be omitted.

***

Quick navigation

* [Example of the structure](#get-example)
* [Root structure](#get-struct)
    * [Filtering results](#get-filters)
        * [By date](#get-filters-dates)
            * [By creation date](#get-filters-dates-created)
        * [By location](#get-location)
    * [Order state](#get-mode)

***

#### <a name="get-example"/>Example of the structure

In the following example every filter is set, this is done completely for illustration,
most likely you won't need all of them in one request. So, as you can guess it, pass only the nodes you need.

```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "order",
  "action": "get",
  "params": {
    "filters": {
      "dates": { // date filter, all filters inside this object considered to be combined by the AND condition 
        "created": { // date of the order finalization
          "from": "2017-05-01", // start of the date diapason for filtering
          "to": "2017-06-15" // end of the date diapason for filtering
        }
      },
      "location": { // all filters inside this object considered to be combined by the AND condition
        "dispatch": "12345678-1234-1234-1234-123456789012", // dispatch location filter
        "destination": "12345678-1234-1234-1234-123456789012" // destination location filter
      },
      "number": [ // can be a single string or an array of any of the following 
        // NOTE: all values inside this array considered to be combined by the OR condition
        "12345678-1234-1234-1234-123456789012", // unique internal order ID
        "700099999", // order number
        "CW23423/dsf332233 (май 2017)" // custom order number, if you specified it in finalization request
      ]
    },
    "limit": 100, // limiting the number of selection results
    "offset": 0, // selection offset
    "mode": { // for more details, see the structure "Order state"
      "isCanceled": false, // does the order have to be canceled to be included in the results
      "isGiven": false, // does the order have to be given out to client to be included in the results
      "isPaid": false, // does the order have to be paid to be included in the results
      "isTaken": true // does the order have to be accepted at the dispatch terminal to be included in the results
    },
    "tab": "onward" // "onward" - On the way, "unpaid" - Speaks for itself, more details in the structure...
  }
}));
```

***

#### <a name="get-struct"/>Root structure. Passed directly into the `params` node

| Structure | Type    | Description                                                                                                                                           |
|-----------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| `filters` | object  | [Order Filter structure](#get-filters)                                                                                                                |
| `limit`   | integer | Limits the number of results returned                                                                                                                 |
| `offset`  | integer | Offsets the number of results returned                                                                                                                |
| `mode`    | object  | [Order State structure](#get-mode). Mutually exclusive with the `tab` node                                                                            |
| `tab`     | string  | A string that sets a predefined order filter. Mutually exclusive with the `mode` node, since it contains pre-configured parameters of the `mode` node |

>Predefined order filters (`tab` node) that are currently available:
>* The `onward` value for the `tab` node defines the following values for the `mode` node:
>```
>{
>  "isCanceled": false, // to be included in results order should not be canceled
>  "isGiven": false // to be included in results order should not be given out to client
>}
>```
>* The `unpaid` value for the `tab` node defines the following values for the `mode` node:
>```
>{
>  "isCanceled": false, // to be included in results order should not be canceled
>  "isPaid": false, // to be included in results order should not be payed at the moment
>  "isTaken": true // to be included in results order should be accepted at the shipping terminal
>}
>```

***

#### <a name="get-filters"/>Order Filter structure. The `params.filters` node

> Part of the [root structure](#get-struct) under the key `filters`.

| Name       | Type          | Description                                                                                                                                                                         |
|------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `dates`    | object        | [Date Filter structure](#get-filters-dates)                                                                                                                                         |
| `customer` | object        | [Customer Filter structure](#get-filters-customer)                                                                                                                                  |
| `location` | object        | [Location Filter structure](#get-filters-location)                                                                                                                                  |
| `number`   | array\|string | A string or array with order numbers (or unique order ID, or custom numbers, that you could specify in order finalization). _In this structure order of the values does not matter_ |

***

#### <a name="get-filters-dates"/>Date Filter structure. The `params.filters.dates` node

> Part of the [Order Filter structure](#get-filters) under the key `dates`.

Currently there is only one date filter node available: `created`. It's responsible for the order creation (finalization) date.

| Name      | Type   | Description                                                  |
|-----------|--------|--------------------------------------------------------------|
| `created` | object | [Creation Date Filter structure](#get-filters-dates-created) |

***

#### <a name="get-filters-dates-created"/>Creation Date Filter structure. The `params.filters.dates.created` node

> Part of the [Date Filter structure](#get-filters-dates) under the key `created`.

| Name   | Type   | Description                                                                                                                                                              |
|--------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `from` | string | A string in `YYYY-MM-DD` format, responsible for the first date of the filtering range by the order creation date and until today, or the day specified in the `to` node |
| `to`   | string | A string in `YYYY-MM-DD` format, responsible for the last date of the filtering range by order creation date                                                             |

***

#### <a name="get-filters-location"/>Location Filter structure. The `params.filters.location` node

> Part of the [Order Filter structure](#get-filters) under the key `location`.

| Name          | Type   | Description                                                                                                            |
|---------------|--------|------------------------------------------------------------------------------------------------------------------------|
| `destination` | string | Unique location identifier. Request returns only those orders where specified location is the **destination** location |
| `dispatch`    | string | Unique location identifier. Request returns only those orders where specified location is the **dispatch** location    |

***

#### <a name="get-mode"/>Order State structure. The `params.mode` node

> Part of the [root structure](#get-struct) under the key `mode`.

Not specifying the order state implies that you are satisfied with both options for the order state.
Specifying `false` insists that orders **having** this state are **excluded** from the result selection.
Specifying `true` insists that orders **not having** this state are **excluded** from the result selection.

| Name         | Type    | Description                                                                                                         |
|--------------|---------|---------------------------------------------------------------------------------------------------------------------|
| `isCanceled` | boolean | An order has to be canceled (`true`) or not canceled (`false`) to be included in the results                        |
| `isGiven`    | boolean | An order has to be given out (`true`) or not given out (`false`) to client to be included in the results            |
| `isPaid`     | boolean | An order has to be payed (`true`) or not payed (`false`) to be included in the results                              |
| `isTaken`    | boolean | An order has to be accepted (`true`) or not accepted (`false`) at a shipping terminal to be included in the results |

***


### <a name="get-response"/>Response data

* [Example of a response](#get-response-example)
* [Description of a response](#get-response-description)

<a name="get-response-example"/>Example of a response:

```javascript
{
  "response": {
    "data": [
      {
        "number": "600331378", // order number
        "id": "3a0421c4-aff4-11e6-80f4-00155d903d0c", // unique order ID
        "status": "Заказ отменен", // order state in plain text
        "dates": {
          "create": "2016-11-21T17:10:24", // when order has been created (finalized)
          "taken": "", // when cargo has been accepted at a shipping terminal
          "shipped": "", // when cargo has been sent from a shipping terminal to a delivery terminal
          "arrived": "", // when cargo has been delivered at a delivery terminal
          "given": "" // when cargo has been given out to a client
        },
        "basePrice": 1020, // base order cost (no discounts)
        "price": 1020, // total order cost (including discounts)
        "service": [ // array of order services
          {
            "name": "Перевозка между городами", // name of the service
            "price": 190 // cost of the service (including discounts)
          },
          {
            "name": "Забор груза от клиента",
            "price": 380
          },
          {
            "name": "Отвоз груза клиенту",
            "price": 450
          }
        ],
        "cargo": {
          "dimension": { // object of cargo dimensions. See "cargo" structure
            "max": {
              "height": 0.1, // max height of a collo
              "length": 0.1, // max length of a collo
              "weight": 0.1, // max weight of a collo
              "width": 0.1 // max width of a collo
            },
            "quantity": 1, // quantity of colli of the cargo
            "volume": 0.1, // general volume of the cargo
            "weight": 0.9 // general weight of the cargo
          }
        },
        "customId": "", // your custom ID if you want to filter by it afterwards (any string)
        "gateway": { // See "gateway" structure
          "dispatch": { // pickup/shipping
            "point": { 
              "location": { 
                "id": "e90f19de-0128-11e5-80c7-00155d903d03", // unique internal location ID
                "name": "Санкт-Петербург", // location name
                "type": "г" // location type (abbreviated)
              },
              "address": "Пример ул., 1", // address
              "date": "2016-11-22", // date of the pickup
              "time": { // time of the pickup
                "start": "09:00", // start of the diapason
                "end": "14:00" // end of the diapason
              }
            },
            "customer": { // contractor (shipper)
              "type": "individual", // not a corporate
              "name": "Арсеньев Сергей Денисович", // name, surname, patronym, if needed
              "phone": [ // up to 3 phone numbers
                "79111883409"
              ],
              "email": "" // e-mail (the more options for a contact the better)
            }
          },
          "destination": { // delivery/receiving point
            "point": {
              "location": {
                "id": "e90f1820-0128-11e5-80c7-00155d903d03",
                "name": "Москва",
                "type": "г"
              },
              "address": "Большой пр-т, 18",
              "date": "2016-11-23", // date of the delivery
              "time": { // time of the delivery
                "start": "14:00",
                "end": "19:00"
              }
            },
            "customer": { // contractor (receiver)
              "type": "individual",
              "name": "Арсеньева Светлана Дмитриевна",
              "phone": [
                "79111883409"
              ],
              "email": ""
            }
          }
        }
      }
      // here can be more orders with the similar structure
    ],
    "meta": { // meta data
      "limit": 100, // limiting the number of orders returned
      "offset": 0, // offset in the array of orders returned
      "total": 1 // total count of orders satisfying the filtering conditions
    }
  }
}
```

<a name="get-response-description"/>Description of a response:

| Name                                                   | Type    | Description                                                                                                                                                                                                                                                |
|--------------------------------------------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `basePrice`                                            | integer | Basic cost of the order (excluding discounts)                                                                                                                                                                                                              |
| `customId`                                             | string  | Custom order number, if specified during creation (finalization)                                                                                                                                                                                           |
| [`dates`](../structure/dates.md)                       | object  | Object that contains dates for order stages. See [Dates of order stage changes](../structure/dates.md)                                                                                                                                                     |
| `documentList`                                         | array   | List of available documents. The key is the document code, the value is the direct link to the document (these links can be transferred to third parties for use; your API token is not required there, keep it safe). The full list of documents is below |
| `cargo`.[`dimension`](../structure/cargo.md#dimension) | object  | Cargo dimensions object. See [Cargo Dimension structure](../structure/cargo.md#dimension)                                                                                                                                                                  |
| `cargo`.`insurance`                                    | integer | Declared cost of cargo insurance. The value is the cost and if it's set, this insurance considered selected                                                                                                                                                |
| `cargo`.`insuranceNdv`                                 | boolean | Insurance with no declared value. `false` for turning it off, `true` by default                                                                                                                                                                            |
| `gateway`                                              | object  | Object of the structure [Gateway](../structure/gateway.md), with the exception of the `service` node, services are returned in the root `service` node (see below)                                                                                         |
| `id`                                                   | string  | Unique internal order ID                                                                                                                                                                                                                                   |
| `number`                                               | integer | Order number                                                                                                                                                                                                                                               |
| `price`                                                | integer | Total cost (including discounts)                                                                                                                                                                                                                           |
| `service`                                              | array   | An array of services, indicating the name and cost (including discounts)                                                                                                                                                                                   |
| `service[n].name`                                      | string  | Name of the service                                                                                                                                                                                                                                        |
| `service[n].price`                                     | integer | Cost of the service (including discounts)                                                                                                                                                                                                                  |
| `status`                                               | object  | Object that contains the current order state                                                                                                                                                                                                               |
| `status.code`                                          | string  | Order state code                                                                                                                                                                                                                                           |
| `status.message`                                       | string  | Order state description (plain text representation of the code)                                                                                                                                                                                            |

List of the documents:

| Code          | Description                                                                     |
|---------------|---------------------------------------------------------------------------------|
| `cargoPhoto`  | Cargo photo(s) (archive)                                                        |
| `cmr`         | Cargo Movement Request, international waybill                                   |
| `doc`         | Order form                                                                      |
| `invoice`     | Order invoice                                                                   |
| `markingList` | Marking sheet, labeling sheet                                                   |
| `lrc`         | Lesser Receipt Certificate, online version of Forwarders Certificate of Receipt |
| `scn`         | Scan of a consignation note (issue slip, delivery order)                        |
| `ssd`         | Scans of accompanying documents                                                 |
| `utd`         | Universal Transfer Document, integrated delivery note                           |


### <a name="get-example-full"/>Examples

**Request**

```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "order",
  "action": "get",
  "params": {
    "filters": {
      "number": "МойПользовательскийНомер"
    }
  }
}));
```

**Response**

```javascript
{
  "response": {
    "data": [
      {
        "number": "700247224",
        "id": "40a18ac7-46d0-11e7-80f9-00155d189b14",
        "status": "Заказ оформлен",
        "dates": {
          "create": "2017-06-01T16:43:18",
          "taken": "",
          "shipped": "",
          "arrived": "",
          "given": ""
        },
        "documentList": {
          "cargoPhoto": "https://vozovoz.ru/api/document/?orderId=40a18ac7-46d0-11e7-80f9-00155d189b14&docType=cargoPhoto",
          "cmr": "https://vozovoz.ru/api/document/?orderId=40a18ac7-46d0-11e7-80f9-00155d189b14&docType=cmr",
          "doc": "https://vozovoz.ru/api/document/?orderId=40a18ac7-46d0-11e7-80f9-00155d189b14&docType=doc",
          "invoice": "https://vozovoz.ru/api/document/?orderId=40a18ac7-46d0-11e7-80f9-00155d189b14&docType=invoice",
          "markingList": "https://vozovoz.ru/api/document/?orderId=40a18ac7-46d0-11e7-80f9-00155d189b14&docType=markingList",
          "scn": "https://vozovoz.ru/api/document/?orderId=40a18ac7-46d0-11e7-80f9-00155d189b14&docType=scn",
          "ssd": "https://vozovoz.ru/api/document/?orderId=40a18ac7-46d0-11e7-80f9-00155d189b14&docType=ssd",
          "utf": "https://vozovoz.ru/api/document/?orderId=40a18ac7-46d0-11e7-80f9-00155d189b14&docType=utd" 
        },
        "basePrice": 2440,
        "price": 2440,
        "service": [
          {
            "name": "Страхование груза без объявленной стоимости",
            "price": 35
          },
          {
            "name": "Жесткий короб",
            "price": 800
          },
          {
            "name": "Упаковка в коробку (20х20х40)",
            "price": 140
          },
          {
            "name": "Перевозка между городами",
            "price": 495
          },
          {
            "name": "Забор груза от клиента",
            "price": 380
          },
          {
            "name": "Отвоз груза клиенту",
            "price": 590
          }
        ],
        "cargo": {
          "dimension": {
            "max": {
              "height": 0.1,
              "length": 0.1,
              "weight": 0.1,
              "width": 0.9
            },
            "quantity": 1,
            "volume": 0.1,
            "weight": 0.9
          },
          "insurance": 590880.23,
          "wrapping": {
            "hardBoxVolume": 0.1,
            "box1": 2
          }
        },
        "customId": "МойПользовательскийНомер",
        "gateway": {
          "dispatch": {
            "point": {
              "location": {
                "id": "e90f19de-0128-11e5-80c7-00155d903d03",
                "name": "Санкт-Петербург",
                "type": "г"
              },
              "address": "Пивоварная ул. 11",
              "date": "2017-06-03",
              "time": {
                "start": "10:00",
                "end": "16:00"
              }
            },
            "customer": {
              "type": "individual",
              "name": "Name 1",
              "phone": [
                "79991112233"
              ],
              "email": ""
            }
          },
          "destination": {
            "point": {
              "location": {
                "id": "e90f1820-0128-11e5-80c7-00155d903d03",
                "name": "Москва",
                "type": "г"
              },
              "address": "Самоварная ул., 1",
              "date": "2017-06-07",
              "time": {
                "start": "10:00",
                "end": "16:00"
              }
            },
            "customer": {
              "type": "individual",
              "name": "Name 2",
              "phone": [
                "79998887766"
              ],
              "email": ""
            }
          }
        },
        "payer": {
          "type": "individual",
          "name": "Name 2",
          "phone": [
            "79998887766"
          ],
          "email": ""
        }
      }
    ],
    "meta": {
      "limit": 100,
      "offset": 0,
      "total": 1
    }
  }
}
```


## <a name="set"/>Creating (finalizing) or updating order. Action `set`

### Request data

* [Example of a structure](#set-example)
* [Root structure](#set-struct)

***

#### <a name="set-example"/>Example of a structure

```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "order",
  "action": "set",
  "params": {
    "cargo": {
      "category": "12345678-1234-1234-1234-1234567890ab", // unique internal category ID, optional
      "correspondence": true, // has correspondence been included (additional safe package with the documents)
      "dimension": { // cargo dimensions
        "max": { // maximum dimensions of a collo
          "height": 0.5, // max height of a collo
          "length": 0.5, // max length of a collo
          "weight": 5.3, // max weight of a collo
          "width": 0.5 // max width of a collo
        },
        "quantity": 2, // quantity of colli of the cargo
        "volume": 0.9, // general volume of the cargo
        "weight": 5.3 // general weight of the cargo
      },
      "insurance": 1000, // declared cost of cargo insurance (if needed, omit it otherwise)
      "insuranceNdv": false, // if you need to turn off insurnace with no declared value (cost) for a payer which is individual customer (will be turned off automatically if you use standard insurance (the one with declared cost))
      "wrapping": { // packaging the cargo
        "bag1": 1, // 1 pc. of wrapping with the `bag1` code
        "box1": 2, // 2 pcs. of wrapping with the `box1` code
        "palletCollar": 0.5 // 0.5m^3 of volumetric wrapping with the `palletCollar` code
      }
    },
    "customId": "MW0123456/ZX15", // custom user ID, defined by you. Optional
    "gateway": { // settings of shipping and receiving
      "dispatch": { // pickup/shipping
        "point": {
          "location": "Санкт-Петербург", // dispatch location
          "address": "Луначарского ул., 7", // dispatch address
          "date": "2017-05-01", // date of the pickup
          "time": { // datetime diapason for the pickup
            "start": "14:00",
            "end": "18:00"
          }
        },
        "service": { // services you want to use in the order
          "needLoading": { // loading service
            "floor": 21, // 21st floor
            "hasLift": true // there's cargo elevator (lift) in the building
          },
          "specificLoading": [ // specific type transport
            "openMachine" // The client is provided with a vehicle with an open body (only sides, no awning)
          ],
          "retrieveAD": { // return accompanying documents, note that this node is also a `point` node
            "location": "Выборг", // location to deliver documents to
            "address": "Просвещения пр-т" // address to deliver documents to
          }
        },
        "customer": { // customer (shipper)
          "email": "customer@ya.ru", // e-mail
          "id": "ivan", // temporary ID for this request. Optional. `dispatch` by default
          "type": "individual", // not a corporate customer
          "name": "Иванов Иван Иванович", // surname, name, patronym
          "phone": "79999999999" // phone number for a contact
        }
      },
      "destination": { // delivery
        "point": {
          "location": "Москва", // delivery location
          "terminal": "1c9871-e09d-df43423-1234-123456", // unique internal terminal ID
          "date": "2017-05-08" // delivery date
        },
        "service": { // services
          "needLoading": { // offloading (won't be used for a delivery terminal, so it's here for an example)
            "used": false // do not include this service
          },
          "specificLoading": [ // specific type transport
            "tailLift"
          ]
        },
        "customer": { // customer (receiver)
          "type": "corporation", // corporate customer
          "companyName": "АЛЬЯНС", // company name
          "name": "Ivanov Ilya", // name, surname of a contact person
          "phone": [ // phone numbers' list
            "79999999999",
            "79998888888"
          ],
          "kpp": "111111111", // reason code
          "inn": "222222222222" // tax ID
        }
      }
    },
    "payer": "ivan", // sets the dispatch customer as payer (by temporary ID). Can be omitted if payer is a receiver customer
    "promoCode": "promo" // promo code, if available
  }
}));
```

#### <a name="get-struct"/>Root structure. Passed directly into the `params` node

| Structure      | Type           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Required**   |   
| `cargo`        | object         | ["Cargo" structure](../structure/cargo.md) _(on another page)_                                                                                                                                                                                                                                                                                                                                                                                   |
| `gateway`      | object         | ["Gateway" structure](../structure/gateway.md) _(on another page)_                                                                                                                                                                                                                                                                                                                                                                               |
| `payer`        | string\|object | String with a temporary ID specified (or not specified) for counterparties in the "Gateway" structure. The default value is `destination`, that means receiver of the cargo is the payer (for example, if you want set receiver as the payer without temporary ID, set it to `destination`). **Can also be a third party**, then contents of this node must be an instance of [Customer structure](../structure/customer.md) _(on another page)_ |
| **Optional**   | 
| `cod`          | object         | [Cash-on-delivery structure](../structure/cod.md) _(on another page)_                                                                                                                                                                                                                                                                                                                                                                            |
| `customId`     | string         | Order ID, defined by user. Ypu can use it in the filters to get this order. Optional. In any case each order has a unique public order number and a unique internal ID                                                                                                                                                                                                                                                                           |
| `promoCode`    | string         | Promo code, if available. Optional                                                                                                                                                                                                                                                                                                                                                                                                               |

### <a name="get-response"/>Response data

* [Example of a response](#set-response-example)
* [Description of a response](#set-response-description)

<a name="set-response-example"/>Example of a response

```javascript
{
  "response": {
    "id": "3afda4a6-46cf-11e7-80f9-00155d189b14", // unique internal order ID
    "number": "700247223", // order number
    "basePrice": 2440, // base cost of the order (excluding discounts)
    "price": 2440, // total cost of the order (including discounts)
    "service": [
      {
        "name": "Перевозка между городами", // name of the service
        "price": 495 // cost of the service (including discounts)
      },
      {
        "name": "Жесткий короб",
        "price": 800
      },
      {
        "name": "Упаковка в коробку (20х20х40)",
        "price": 140
      },
      {
        "name": "Страхование груза без объявленной стоимости",
        "price": 35
      },
      {
        "name": "Забор груза от клиента",
        "price": 380
      },
      {
        "name": "Отвоз груза клиенту",
        "price": 590
      }
    ]
  }
}
```

<a name="set-response-description"/>Description of a response

| Name             | Type    | Description                                                              |
|------------------|---------|--------------------------------------------------------------------------|
| id               | string  | Unique internal order ID                                                 |
| number           | integer | Order number                                                             |
| basePrice        | integer | Basic cost of the order (excluding discounts)                            |
| price            | integer | Total cost of the order (including discounts)                            |
| service          | array   | An array of services, indicating the name and cost (including discounts) |
| service[n].name  | string  | Name of the service                                                      |
| service[n].price | integer | Cost of the service (including discounts)                                |

### <a name="set-example-full"/>Examples

**Request**

```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "order",
  "action": "set",
  "params": {
    "cargo": {
      "dimension": {
        "volume": 0.1,
        "weight": 0.9,
      },
      "wrapping": {
        "box1": 2,
        "hardBoxVolume": 0.5
      }
    },
    "gateway": {
      "dispatch": {
        "point": {
          "location": "Санкт-Петербург",
          "address": "Пивоварная ул. 11",
          "date": "2017-06-03",
          "time": {
            "start": "10:00",
            "end": "16:00"
          }
        },
        "customer": {
          "name": "Name 1",
          "phone": "79092407718"
        }
      },
      "destination": {
        "point": {
          "location": "Москва",
          "address": "Ячменный пер., 69",
          "date": "2017-06-07",
          "time": {
            "start": "10:00",
            "end": "16:00"
          }
        },
        "customer": {
          "id": "name2",
          "name": "Name 2",
          "phone": "79092407700"
        }
      }
    },
    "payer": "name2"
  }
}));
```

**Response**

```javascript
{
  "response": {
    "id": "3afda4a6-46cf-11e7-80f9-00155d189b14", // unique internal order ID
    "number": "700247223", // order number
    "basePrice": 2440, // base cost of the order
    "price": 2440, // total cost of the order
    "service": [
      {
        "name": "Перевозка между городами", // name of the service
        "price": 495 // cost of the service
      },
      {
        "name": "Жесткий короб",
        "price": 800
      },
      {
        "name": "Упаковка в коробку (20х20х40)",
        "price": 140
      },
      {
        "name": "Страхование груза без объявленной стоимости",
        "price": 35
      },
      {
        "name": "Забор груза от клиента",
        "price": 380
      },
      {
        "name": "Отвоз груза клиенту",
        "price": 590
      }
    ]
  }
}
```

## <a name="cancel"/>Canceling an order

Represents an object used to cancel a placed order.

**Request**

```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "order",
  "action": "cancel",
  "params": {
    "id": "3afda4a6-46cf-11e7-80f9-00155d189b14", // unique internal order ID
    "reason": "Тестовый заказ. Проверяли работоспособность" // reason of canceling an order in free form
  }
}));
```

**Response**

```javascript
{
  "response": {
    "message": "Заказ успешно отменён" // if error occured, there will be an error object
  }
}
```

***
[▲ Up](#up)
