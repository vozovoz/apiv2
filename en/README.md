Vozovoz API 2.5
===============

[Версия на русском](../README.md)

---------------

Vozovoz API is an interface for interacting with https://vozovoz.ru.
We're using HTTPS as transfer protocol.
You can test API methods at [demonstration server](https://vozovoz.org/dev/api/).

## Contents

* [Quick start](docs/quick.md)
    * [Order cost calculation](docs/object/price.md)
    * [Order finalization](docs/object/order.md#set)
* [Request parameters](docs/params/index.md)
    * [Authentication](docs/params/auth.md)
* [Objects](docs/object/index.md)
    * [Cost](docs/object/price.md)
    * [Delivery schedule](docs/object/schedule.md)
    * [Direct query](docs/object/directQuery.md)
    * [Location](docs/object/location.md)
    * [Order](docs/object/order.md)
        * [Get (information about order setup)](docs/object/order.md#get)
        * [Set (finalize order setup)](docs/object/order.md#set)
    * [Terminal](docs/object/terminal.md)
    * [Wrapping](docs/object/wrapping.md)
* [Structures](docs/structure/index.md)
    * [Cargo](docs/structure/cargo.md)
        * [Dimensions](docs/structure/cargo.md#dimension)
        * [Wrapping](docs/structure/cargo.md#wrapping)
    * [Customer](docs/structure/customer.md)
    * [Gateway](docs/structure/gateway.md)
        * [Point](docs/structure/gateway.md#point)
        * [Services](docs/structure/service.md)

***

If you have any questions, please write a letter to [api@vozovoz.ru](mailto:api@vozovoz.ru).