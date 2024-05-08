# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](../index.md) > [Direct query](../directQuery.md) > Cargo categories

> **Object code: `directQuery`**<br/>
> **Method code: `getCargoTypes`**


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
    "method": "getCargoTypes",
    "data": {
      "limit": 5,
      "offset": 0
    }
  }
}));
```

<a name="response-example"/>**Response**

```javascript
{
  "response": {
    "data": [ // result array
      {
        "id": "f1965e6a-13ea-11e4-826b-d850e6bbb0fc", // unique internal category ID (it's used when placing an order)
        "name": "Автозапчасти кузовные", // name of the category
        "sortIndex": 0, // sorting index
        "category": { // parent category data
          "id": "04776e4f-8e19-11e5-80d2-00155d903d03", // unique internal category ID (you can also use this ID when placing an order)
          "name": "Авто-мото-вело техника" // name of the category
        },
        "restrictions": { // restrictions of the category
          "declaredCost": { // declared cost (for insurance)
            "min": 0, // minimum declared value
            "max": 30000000 // maximum declared value
          }
        }
      },
      {
        "id": "2d80ef4a-b81e-43f1-bc75-06ca377406da",
        "name": "Сборный груз",
        "sortIndex": 0,
        "category": {
          "id": "b55b0c18-8e19-11e5-80d2-00155d903d03",
          "name": "Сборный груз"
        },
        "restrictions": {
          "declaredCost": {
            "min": 0,
            "max": 30000000
          }
        }
      },
      {
        "id": "f1965e6b-13ea-11e4-826b-d850e6bbb0fc",
        "name": "Аккумуляторы незаряженные",
        "sortIndex": 0,
        "category": {
          "id": "04776e4f-8e19-11e5-80d2-00155d903d03",
          "name": "Авто-мото-вело техника"
        },
        "restrictions": {
          "declaredCost": {
            "min": 0,
            "max": 30000000
          }
        }
      },
      {
        "id": "f1965e6c-13ea-11e4-826b-d850e6bbb0fc",
        "name": "Бытовая техника без зав. упаковки",
        "sortIndex": 0,
        "category": {
          "id": "04776e50-8e19-11e5-80d2-00155d903d03",
          "name": "Бытовая техника и электроника"
        },
        "restrictions": {
          "declaredCost": {
            "min": 0,
            "max": 30000000
          }
        }
      },
      {
        "id": "f1965e6d-13ea-11e4-826b-d850e6bbb0fc",
        "name": "Выставочное оборудование",
        "sortIndex": 0,
        "category": {
          "id": "00000000-0000-0000-0000-000000000000", // parent category is absent
          "name": null
        },
        "restrictions": {
          "declaredCost": {
            "min": 0,
            "max": 30000000
          }
        }
      }
    ],
    "meta": { // meta data
      "limit": 5, // specified limit
      "offset": 0, // specified offset
      "total": 73 // total count of entries returned
    }
  }
}
```


## <a name="description"/>Descripton

The [direct query](../directQuery.md) object using `getCargoTypes` method returns the list of cargo categories.


### Request data

| Structures | Type   | Description                                                              |
|------------|--------|--------------------------------------------------------------------------|
| Required   |   
| `method`   | string | The name of the method to call. Should be `getCargoTypes`                |
| Optional   | 
| `data`     | object | Data required for the called method to operate. Optional for this method |

>The `data` node may contain:
>* `limit` (positive integer) - limit of entries returned
>* `offset` (positive integer) - offset for number of entries returned


### Response data

>The response data is described as comments in [example above](#response-example).


***
[▲ Up](#up)