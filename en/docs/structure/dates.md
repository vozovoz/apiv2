# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Request data structures](index.md) > Order step dates

## Contents

* [Example](#example)
* [Description](#description)
* [Where is used](#used)

## <a name="example"/>Example

```javascript
{
  "dates": {
    "created": "2017-03-16T16:49:36",
    "taken": "2017-03-17T11:38:45",
    "shipped": "2017-03-17T18:00:00",
    "arrived": "2017-03-18T15:00:00",
    "given": ""
  }
}
```

## <a name="description"/>Description

| Node      | Type   | Description                                    |
|-----------|--------|------------------------------------------------|
| `created` | string | Date of order creation                         |
| `taken`   | string | Date of cargo reception at a shipping terminal |
| `shipped` | string | Date of shipping from a shipping terminal      |
| `arrived` | string | Date of delivery at a warehouse terminal       |
| `given`   | string | Date of receiving shipment by a customer       |


## <a name="used"/>Where is used

| Object  | Action | Description                              |
|---------|--------|------------------------------------------|
| `order` | `get`  | Comes as one of root nodes in a response |

***
[▲ Up](#up)