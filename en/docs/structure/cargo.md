# <a name="up"/>Vozovoz API 2.5

[Main page](/en/README.md) > [Request data structures](index.md) > Cargo

## Contents

* [Data structure](#struct)
    * [`cargo` structure](#cargo)
        * [`dimension` structure](#dimension)
            * [`max` structure (Maximal dimensions of a collo)](#max)
        * [`wizard` structure (Set cargo by describing every collo)](#wizard)
        * [`wrapping` structure](#wrapping)
* [Examples](#example)


## <a name="struct"/>Data structure

>In this manual for easier understanding a word **_structures_** will be used
to name any structured data (mostly that contains other structures inside,
but also that contains simple data types, like string, number, boolean).
We name **full** necessary data object, that used in a request to our server
&mdash; ["parameters"](../params/index.md). Key to pass "parameters" into a request
called "params". More about that [here](../params/post.md). And we name **_nodes_**
certain structures with a defined key, that this structure is assigned to.


### <a name="cargo"/>`cargo` structure

| Name                      | Type    | Description                                                                                                                                                                                                  |
|---------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `category`                | string  | Contains unique identificator of this cargo's category. _Optional node (used only for actions with an order)_                                                                                                |
| `correspondence`          | boolean | Does this cargo contains correspondence, a safe package with any documents. _Optional node (`false` by default)_                                                                                             |
| [`dimension`](#dimension) | object  | Contains data about this cargo's dimensions. Can be omitted, if `"correspondence": true`                                                                                                                     |
| `insurance`               | integer | Sets declared value of the cargo insurance (in Russian rubles only) _Optional node_                                                                                                                          |
| `insuranceNdv`            | boolean | Sets the state of an insurance of non-declared value type. Turns off automatically, if you use a declared value insurance (can be omitted), otherwise forcibly turns on if a payer of the order is a company |
| [`wizard`](#wizard)       | object  | Contains dimension descriptions collo-by-collo (replaces `dimension` node). _Optional node_                                                                                                                  |
| [`wrapping`](#wrapping)   | object  | Contains wrapping data structure (both general volumetric and quantity-related ones). _Optional node_                                                                                                        |

Example

```javascript
{
  "category": "12345678-1234-1234-1234-1234567890ab",
  "correspondence": true,
  "dimension": { /* for more information see "Dimension" structure */ },
  "insurance": 6500,
  "wrapping": { /* for more information see "Wrapping" structure */ }
}
```


### <a name="dimension"/>`dimension` structure

>Every dimension is being accepted the same as on the website: volume in cubic meters, weight in kilograms.

| Name           | Type    | Description                                                                                                                            |
|----------------|---------|----------------------------------------------------------------------------------------------------------------------------------------|
| [`max`](#max)  | object  | Structure that defines maximal dimensions of a collo. Required in case cargo is over size (OOG) or general dimensions has not been set |
| `quantity`     | integer | Positive integer that defines **quantity of colli**. _Optional. Default is **1** (piece)_                                              |
| `volume`       | float   | Positive fractional number that defines **general volume** of a cargo. _Optional, if maximal dimensions has been set (m<sup>3</sup>)_  |
| `weight`       | float   | Positive fractional number that deinfes **general weight** of a cargo. _Optional, if maximal dimensions has been set (kg)_             |

Example

```javascript
{
  "max": {
    "height": 0.2,
    "length": 0.4,
    "weight": 12.4,
    "width": 0.1
  },
  "quantity": 2,
  "volume": 0.01,
  "weight": 24.8 // general weight cannot be less than maximal weight of a collo
}
```

### <a name="max"/>`max` structure (Maximal dimensions of a collo)

| Name     | Type  | Description                                                                |
|----------|-------|----------------------------------------------------------------------------|
| `height` | float | Maximal height of a collo from all the colli in the order _(in meters)_    |
| `length` | float | Maximal length of a collo from all the colli in the order _(in meters)_    |
| `weight` | float | Maximal weight of a collo from all the colli in the order _(in kilograms)_ |
| `width`  | float | Maximal width of a collo from all the colli in the order _(in meters)_     |

Example

```json
{
  "height": 0.2,
  "length": 0.4,
  "weight": 12.4,
  "width": 0.1
}
```

### <a name="wizard"/>`wizard` structure (Set cargo by describing every collo)

Example

```javascript
{
  "wizard": [
    { // collo 1
      "length": 0.3,
      "width": 0.2,
      "height": 0.1,
      "quantity": 2, // this means it's 2 colli with these dimensions (length, width, height, weight)
      "weight": 10,
      "wrapping": {  // volumetric wrapping only
          "bubbleFilmVolume": 0.5
      }
    },
    { // collo 2
      "length": 0.5,
      "width": 0.1,
      "height": 0.5,
      "quantity": 3, // this means it's 3 colli with these dimensions (length, width, height, weight)
      "weight": 8,
      "wrapping": {} // volumetric wrapping only
    }
  ]
}
```

### <a name="wrapping"/>`Wrapping` structure

> More about wrappings' codes in ["Wrapping" object](../object/wrapping.md).

Example

```javascript
"wrapping": {
  box1: 2, // 2 pcs. of the box with code `box1`
  bubbleFilmVolume: 0.5, // 0.5m^3 - volume of a bubble film, that will be used as a wrapping
}
```

## <a name="example"/>Examples

```javascript
// full javascript code to use in web browser developer console, see in "Quick start" section
xhttp.send(JSON.stringify(
{
  "object": "price",
  "action": "get",
  "params": {
    "cargo" : {
      "dimension": {
        "max": {
          "length": 0.1, // maximal length of a collo
          "height": 0.1, // maximal height of a collo
          "weight": 0.9, // maximal weight of a collo
          "width": 0.1 // maximal width of a collo
        },
        "quantity": 1, // quantity of colli
        "volume": 0.01, // general volume
        "weight": 0.9 // general weight
      }
    },
    "gateway": {
      "dispatch": { // where from
        "point": {
          "location": "Санкт-Петербург",
          "terminal": "default" // terminal by default
        }
      },
      "destination": { // where to
        "point": {
          "location": "Москва",
          "terminal": "default" // terminal by default
        }
      }
    }
  }
}));
```

```javascript
xhttp.send(JSON.stringify(
{
  "object": "price",
  "action": "get",
  "params": {
    "cargo" : {
      "wizard": [ // exact info for each colli
        { // collo 1
          "length": 0.3, // length of the collo
          "width": 0.2, // width of the collo
          "height": 0.1, // height of the collo
          "quantity": 2, // quantity of the colli with the same dimensions,
          // in other words, if dimensions are the same, you just write here quantity of such colli
          "weight": 10, // weight of the collo
          "wrapping": { // here used only volume wrapping
            "bubbleFilmVolume": 0.5
          }
        },
        { // collo 2 (other dimensions)
          "length": 0.5,
          "width": 0.1,
          "height": 0.5,
          "quantity": 3,
          "weight": 8,
          "wrapping": {} // here used only volume wrapping
        }
      ]
    },
    "gateway": {
      "dispatch": { // where from
        "point": {
          "location": "Санкт-Петербург",
          "terminal": "default" // a terminal by default
        }
      },
      "destination": { // where to
        "point": {
          "location": "Москва",
          "terminal": "default" // a terminal by default
        }
      }
    }
  }
}));
```

***
[▲ Up](#up)