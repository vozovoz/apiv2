# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Request data structures](index.md) > Services

## Contents

* [Example](#example)
* [Description](#description)
    * [Loading/Offloading](#needLoading)
    * [Specific transport](#specLoading)
    * [Return accompanying documents](#retrieveAD)
* [Where is used](#used)

## <a name="example"/>Example

```javascript
{
  "needLoading": { // "loading" service for `dispatch` or "offloading" for `destination`
    "floor": 12, // floor where loading/offloading is needed
    "lift": true, // sets if there's elevator in a building
  },
  "specificLoading": [ // "specific transport" service
    'openMachine' // unique code of a transport vehicle or service
  ],
  "retrieveAD": { // "return accompanying documents" service (available only for dispatch)
    "location": "Санкт-Петербург", // location, where to return to (you can also use unique internal ID)
    "terminal": "default" // terminal, where to return to (you can also use unique internal ID)
    // or use "address": "street, house"
  },
  "unboxingOnDelivery" : 20 // "unboxing package upon address delivery" service
  // the value is the weight of the cargo, that needs unboxing, in kilograms
}
```

## <a name="description"/>Description

| Node                              | Type    | Available for                           | Description                                                                                                               |
|-----------------------------------|---------|-----------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| [`needLoading`](#needLoading)     | object  | `dispatch` and `destination` both       | "Loading" service for pickup at a particular address or "Offloading" for delivery to a particular address                 |
| [`specificLoading`](#specLoading) | array   | `dispatch` and `destination` both       | "Specific transport" service. Array of codes, that represent types of transport needed for "loading"/"offloading" service |
| [`retrieveAD`](#retrieveAD)       | object  | `dispatch` only                         | "Return accompanying documents" service                                                                                   |
| [`scannedConsignationNote`](#scn) | boolean | `dispatch` only                         | Scan of a consignation note                                                                                               |
| [`unboxingOnDelivery`](#uod)      | float   | `dispatch` or `destination` exclusively | Weight of the cargo after unboxing (kg) for "Unboxing on delivery" service                                                |

### <a name="needLoading"/>Loading/Offloading

Used only when pickup/delivery option is `address`.

| Node    | Type    | Description                                                                                                                     |
|---------|---------|---------------------------------------------------------------------------------------------------------------------------------|
| `floor` | integer | The floor where cargo will be picked up for shipping or received respectively                                                   |
| `lift`  | boolean | Is there elevator/lift to use for loading/offloading service. `false` by default _(no lift)_                                    |
| `used`  | boolean | Is the service is 'on' or 'off'. Can be omitted, because is considered `true`, if the `needLoading` node passed in the request. |

### <a name="specLoading"/>Specific transport

Used only when pickup/delivery option is `address`. The array can contain the following codes:

| Code                | Short description | Full description                                                                                                                               |
|---------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| `lateralLoad`       | Lateral loading   | During loading and unloading operations, it must be possible to open the body from the side (right or left) of the vehicle.                    |
| `manipulator`       | Articulated arm   | The vehicle must be equipped with a crane with a boom outreach of at least 8 meters and a lifting capacity of at least 1 ton at this outreach. |
| `openMachine`       | Pick up truck     | The client is provided with a vehicle with an open body (only sides, no awning).                                                               |
| `removableCurtains` | Removable awning  | During loading and unloading operations, the awning is removed from the vehicle body.                                                          |
| `tailLift`          | Tail lift         | The vehicle must be equipped with a lifting mechanism (shovel-hoisting) with a lifting capacity of at least 500 kg.                            |
| `topLoad`           | Removable top     | During loading and unloading operations, it must be possible to open the top of the vehicle's body for cargo loading using a lifting crane.    |

> In most cases one type excludes another, but some types don't. Ask us any questions at [api@vozovoz.ru](mailto://api@vozovoz.ru).

### <a name="retrieveAD"/>Return accompanying documents

>Please note that by default, the data for this service is taken from the dispatch point. Therefore, if you okay with these values, you can specify:
>```
>{
>  // place other services here or below, if you need any
>  "retrieveAD": {}
>  // below
>}
>```
>Or for a simplicity in this way:
>```
>{
>  // place other services here or below, if you need any
>  "retrieveAD": {
>    "used": true
>  }
>  // below
>}

| Node       | Type    | Description                                                                                                                                                                           |
|------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `address`  | string  | Must be a string with the required address relative to the specified location. Should be omitted, if a return A.D. is required to a terminal.                                         |
| `location` | string  | Can be either a unique internal location ID or a query string, for example, "Санкт-Петербург"                                                                                         |
| `terminal` | string  | Can be either a unique internal terminal ID or a `default` value to select the default terminal of the specified location. Should be omitted if a return A.D. is required to address. |
| `scanned`  | boolean | Set to `true` in case you also need scanned copies of accompanying documents. `false` by default.                                                                                     |
| `used`     | boolean | Is the service is 'on' or 'off'. Can be omitted, because is considered `true`, if the `retrieveAD` node passed in the request.                                                        |

>If `address` and `terminal` nodes are specified simultaneously, `address` takes precedence. Carefully check the request structure before sending!
>
>Any of the specified nodes can be omitted (not specified), the values for it will be taken from the dispatch point `gateway.dispatch.point`

### <a name="scn"/>Scan of consignation note

>```
>{
>  "scannedConsignationNote": true
>}

| Node                      | Type    | Description                                          |
|---------------------------|---------|------------------------------------------------------|
| `scannedConsignationNote` | boolean | Is the service is 'on' of 'off'. `false` by default. |


### <a name="uod"/>Unboxing on delivery to an address

>```
>{
>  "unboxingOnDelivery": 16.3
>}

| Node                 | Type  | Description                                           |
|----------------------|-------|-------------------------------------------------------|
| `unboxingOnDelivery` | float | Weight of the cargo (in kilograms) after the unboxing |

## <a name="used"/>Where is used

| Object   | Action | Description                                                   |
|----------|--------|---------------------------------------------------------------|
| `order`  | `set`  | Can be presented in `gateway.dispatch`, `gateway.destination` |
| `price`  | `get`  | Can be presented in `gateway.dispatch`, `gateway.destination` |

***
[▲ Up](#up)