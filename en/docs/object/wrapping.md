# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](index.md) > Wrapping


> **Object code: `wrapping`**

## Contents

* [Description](#description)
* [Getting data for one or more wrappings](#get)
    * [General example of the structure](#get-example)
    * [Description of the structure](#get-struct)
    * [Response data](#get-response)
        * [Example of a response](#get-response-example)
        * [Description of a response structure](#get-response-description)


## <a name="description"/>Wrapping

Represents an object used to obtain data of available cargo packages.

Packages are divided into two types: quantitative and volumetric. To pass a `wrapping` structure in a request,
when finalizing an order or requesting a price you must specify the unique packaging code as a key,
and set the value depending on the type of packaging.
For quantitative packages, integer values imply the number of pieces of the specified package,
and for volumetric ones &mdash; volume of cargo (m<sup>3</sup>) that needs to be packed.

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
  "object": "wrapping",
  "action": "get",
  "params": {
    "code": "hardBoxVolume", // specify a certain packaging by code
    // without the parameter above this method will return the full list
    "limit": 0, // 0 is for default limit
    "offset": 0
  }
}
```

#### <a name="get-struct"/>Root structure. Passed directly into `params` node

| Structure   | Type    | Description                                                                                                                    |
|-------------|---------|--------------------------------------------------------------------------------------------------------------------------------|
| `code`      | string  | Specifies the package for which data should be returned. If not specified, the method returns a list of all available packages |
| `limit`     | integer | Limits the number of records returned                                                                                          |
| `offset`    | integer | Sets the offset for the number of records returned                                                                             |


### <a name="get-response"/>Response data

* [Example of a response](#get-response-example)
* [Description of a response data](#get-response-description)

<a name="get-response-example"/>Example of a response:

```javascript
{
  "response": [
    {
      "code": "hardPackageVolume", // unique code of the wrapping
      "name": "Жёсткая упаковка", // name of the wrapping
      "type": "volume", // type of the wrapping is volumetric
      // it means, that passing its values in other requests, these values must be frational positive numbers 
      // that specify the volume of cargo that requires packaging
      "description": "Description for the client in Russian"
    },
    {
      "code": "box3",
      "name": "Коробка 40×40×20 см",
      "type": "numeric", // type of the wrapping is quantitative
      // it means, that passing its values in other requests, these values must be positive integers, 
      // that specify the quantity of these packages
      "description": "Описание для клиента"
    }
    // the number of results depends on their number for this request and the `limit` node
  ]
}
```

<a name="get-response-description"/>Description of a response data:

| Name          | Type   | Description                                                                   |
|---------------|--------|-------------------------------------------------------------------------------|
| `code`        | string | Unique code of the wrapping                                                   |
| `description` | string | Description of the wrapping                                                   |
| `name`        | string | Name (definition) of the wrapping                                             |
| `type`        | string | Type of the wrapping. Can be `numeric` (quantitative) и `volume` (volumetric) |

***
[▲ Up](#up)