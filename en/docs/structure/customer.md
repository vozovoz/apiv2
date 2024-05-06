# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Request data structures](index.md) > Customer (contractor)

## Contents

* [Example](#example)
* [Description](#description)
* [Where is used](#used)

## <a name="example"/>Example

```javascript
{
  "customer": { // or "payer", if you need to describe payer (in case it's a third-person-customer)
    "companyName": "ООО Компания", // name of the company, in case it's a corporate customer
    "email": "example@example.ru", // contact e-mail
    "name": "Иванов Иван Иванович", // name and/or surname of a contact person
    "phone": "79999999999", // phone number as string, or array of phone numbers (strings)
    "sendCode": true, // defines the code sending (SMS)
    "type": "corporation", // in case it's a corporate customer, use "individual" for an individual customer
    "inn": "6009999999", // tax ID, or Individual Tax Number (for corporate customers only, and is required, if payer is corporate)
    "kpp": "525999999" // Reason Code for Tax Registration (for corporate customers only)
  }
}
```

## <a name="description"/>Description

| Node          | Type          | Description                                                                     |
|---------------|---------------|---------------------------------------------------------------------------------|
| `companyName` | string        | Name of the company (corporate only)                                            |
| `email`       | string        | Customer's e-mail                                                               |
| `name`        | string        | Contact person's name and/or credentials                                        |
| `phone`       | array\|string | One phone number or array of phone numbers of the contact person                |
| `sendCode`    | boolean       | Sends message with a receiver's code for the customer, if is set to `true`      |
| `type`        | string        | Customer type. `individual` for a person, `corporation` for corporate customers |
| `inn`         | string        | Individual tax number (corporate only). Required, if payer is corporate         |
| `kpp`         | string        | Reason code for tax registration (corporate only)                               |

## <a name="used"/>Where is used

| Object         | Action       | Description                                                                                                                                    |
|----------------|--------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| `order`        | `get`, `set` | Considered as one of the necessary nodes for [gateway](gateway.md) structure, or a base structure for `payer` node, if payer is a third person |

***
[▲ Up](#up)