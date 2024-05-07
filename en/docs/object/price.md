# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Objects](index.md) > Cost of an order


> **Object code: `price`**


## Contents

* [Examples](#example)
* [Description](#description)


## <a name="example"/>Examples

**Example of the request in JavaScript**
```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "price",
  "action": "get",
  "params": {
    "cargo" : {
      "dimension": { // cargo dimensions
        "quantity": 1, // quantity of colli
        "volume": 0.1, // general volume
        "weight": 0.9 // general weight
      }
    },
    "gateway": {
      "dispatch": { // where from (shipping)
        "point": {
          "location": "Санкт-Петербург",
          "terminal": "default" // default terminal for location "Санкт-Петербург"
        }
      },
      "destination": { // where to (delivery)
        "point": {
          "location": "Москва",
          "terminal": "default" // default terminal for location "Москва"
        }
      }
    }
  }
}));
```

**Example of the response**
```json
{
  "response": {
    "basePrice": 1250,
    "price": 1200,
    "service": [
      {
        "name": "Перевозка между городами",
        "price": 370
      },
      {
        "name": "Страхование груза без объявленной стоимости",
        "price": 35
      },
      {
        "name": "Забор груза от клиента",
        "price": 363
      },
      {
        "name": "Отвоз груза клиенту",
        "price": 432
      }
    ],
    "deliveryTime": {
      "from": 1,
      "to": 1
    }
  }
}
```


## <a name="description"/>Description

Represents an array of services with a specified cost, as well as the total cost with and without discount(s).


### Request data

| Structure   | Type   | Description                                    |
|-------------|--------|------------------------------------------------|
| Required    |
| `cargo`     | object | [`cargo` structure](../structure/cargo.md)     |
| `gateway`   | object | [`gateway` strucutre](../structure/gateway.md) |
| Optional    | 
| `promoCode` | string | Contains optional promo code                   |


### Response data

| Name              | Type    | Description                                                      |
|-------------------|---------|------------------------------------------------------------------|
| basePrice         | integer | Basic cost with no discounts (in most cases in Russian rubles)   |
| price             | integer | Total cost including discounts (in most cases in Russian rubles) |
| service           | array   | Array of services, indicating the name and cost with discounts   |                                                            
| service\[n].name  | string  | Name of the service                                              |
| service\[n].price | integer | Cost of the service with discounts                               |
| deliveryTime      | object  | Delivery time range in days                                      |
| deliveryTime.from | integer | Minimum delivery time in days                                    |
| deliveryTime.to   | integer | Maximum delivery time in days                                    |


***
[▲ Up](#up)