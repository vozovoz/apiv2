# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Request data structures](index.md) > Cash-on-delivery functionality (COD for short)

## Contents

* [Example](#example)
* [Description](#description)
* [Where is used?](#used)

## <a name="example"/>Example

```javascript
{
  "cod": {
    "list": [
      {
        "code": "MY_SHOP_A0232", // stock/product identification number
        "name": "Телевизор", // product name
        "quantity": 4, // quantity of this product
        "price": 99.68 // cost by piece
      },
      {
        "code": "MY_SHOP_B0371", // stock/product identification number
        "name": "Холодильник", // product name
        "quantity": 2, // quantity of this product
        "price": 47.55 // cost by piece
      }
    ],
    "shop": {
      "date": "2020-08-16", // date of the order in the store (will be printed on the bill when checkout)
      "order": "MY-SHOP-2460544 / 16.34233.43" // name of the order in the store (will be printed on the bill when checkout)
    }
  }
}
```


## <a name="description"/>Description

| Node        | Type    | Description                                            |
|-------------|---------|--------------------------------------------------------|
| `list`      | array   | Array of COD objects                                   |
| .`code`     | string  | Product identification number of COD object            |
| .`name`     | string  | Product name of COD object                             |
| .`quantity` | integer | Quantity of the product in this COD object (in pieces) |
| .`price`    | float   | Cost by piece of the product in this COD object        |
| `shop`      | object  | Object with store order data                           |
| .`date`     | string  | Date of the order in the store                         |
| .`order`    | string  | Number of the order in the store                       |


## <a name="used"/>Where is used

| Object  | Action | Description                                                                           |
|---------|--------|---------------------------------------------------------------------------------------|
| `order` | `set`  | Used in the [root structure of the order finalization](../object/order.md#set-struct) |

***
[▲ Up](#up)