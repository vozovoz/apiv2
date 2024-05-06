# <a name="up"/>Vozovoz API 2.5

[Main page](/README.md) > Request data structures

In this manual for easier understanding a word **_structures_** will be used
to name any structured data (mostly that contains other structures inside,
but also that contains simple data types, like string, number, boolean).
We name **full** necessary data object, that used in a request to our server
&mdash; ["parameters"](../params/index.md). Key to pass "parameters" into a request
called "params". More about that [here](../params/post.md). And we name **_nodes_**
certain structures with a defined key, that this structure is assigned to.

## Quick access to basic structures

* [Cargo](cargo.md)
* [Customer](customer.md)
* [Gateway](gateway.md)

## Structures' list _(in alphabet order)_

To get response from any object, have a look at examples of the object. Go to [objects' list](../object/index.md).

| Name                                    | Type           | Being a part of                              | Description                                                                                                                                                        |
|-----------------------------------------|----------------|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [`cargo`](cargo.md)                     | object         | _root element_                               | "cargo" node                                                                                                                                                       |
| [`customer`](customer.md)               | object         | [`dispatch`<br/>`destination`](gateway.md)   | Child node of "gateway" node                                                                                                                                       |
| [`destination`](gateway.md#destination) | object         | [`gateway`](gateway.md)                      | Child node of "gateway" node                                                                                                                                       |
| [`dimension`](cargo.md#dimension)       | object         | [`cargo`](cargo.md)                          | Child node of "cargo" node                                                                                                                                         |
| [`dispatch`](gateway.md#dispatch)       | object         | [`gateway`](gateway.md)                      | Child node of "gateway" node                                                                                                                                       |
| [`gateway`](gateway.md)                 | object         | _root element_                               | "gateway" node                                                                                                                                                     |
| [`max`](cargo.md#max)                   | object         | [`dimension`](cargo.md#dimension)            | Child node of "dimension" node, that is a child node of "cargo" node                                                                                               |
| [`payer`](customer.md)                  | object\|string | _root element_                               | Either string, that contains unique identificator, the one that was set somewhere in the "customer" structure of the same request, or "customer" structure itself. |
| [`point`](gateway.md#point)             | object         | [`dispatch`<br/>`destination`](gateway.md)   | Child node of "gateway" node                                                                                                                                       |
| [`service`](gateway.md#service)         | object         | [`dispatch`<br/>`destination`](gateway.md)   | Child node of "gateway" node                                                                                                                                       |
| [`wrapping`](cargo.md#wrapping)         | object         | [`cargo`](cargo.md)                          | Child node of "cargo" node                                                                                                                                         |

***
[▲ Up](#up)