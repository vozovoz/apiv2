# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Request data structures](index.md) > Documents for the Electronic Transportation Documents Management System

## Contents

* [Example](#example)
* [Description](#description)
* [Where is used](#used)

## <a name="example"/>Example

```metadata json
{
  "attachEtdms": [
    {
      "type": "digitalExpeditingCommission", // document type (digital expediting commission or electronic forwarding instruction)
      "name": "ЭПЭ-00001234" // document name
    }
  ]
}
```


## <a name="description"/>Description

| Node   | Type   | Description |
|--------|--------|-------------|
| `type` | string | Document type. `digitalExpeditingCommission` means an electronic forwarding instruction |
| `name` | string | Document name |


## <a name="used"/>Where is used

| Object  | Action | Description |
|---------|--------|-------------|
| `order` | `set`  | Used in the [root structure of the order finalization](../object/order.md#set-struct) |

***
[▲ Up](#up)
