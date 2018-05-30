## Product List
Query products with pagination(using `Relay Connection`, check https://www.learnrelay.org/connections/cursors-pagination/ for details)

```
query productList(
  $first: Int,
  $after: String,
  $before: String,
  $last: Int,
  $updatedTimeAfter: Int,
  $updatedTimeBefore: Int,
) {
  productList(
    first: $first,
    after: $after,
    before: $before,
    last: $last,
    updatedTimeAfter: $updatedTimeAfter,
    updatedTimeBefore: $updatedTimeBefore,
  ) {
    length
    edges {
      node {
        id
        name
      }
    }
  }
}
```

Filter parameters:

name | type | description
--- | --- | ---
updatedTimeAfter | Int | `unix timestamp`, filter products updated at after this timestamp
updatedTimeBefore | Int | `unix timestamp`, filter products updated at before this timestamp

Connection parameters, more details about collection pagination please visit http://graphql.org/learn/pagination/

name | type | description
--- | --- | ---
first | Int | limit size
last | Int | limit size
after | String | offset cursor
before | String | offset cursor

Product list fields:

field name | type | required | description
--- | --- | --- | ---
length | Int | True | total matches count
edges | List | True | node list
edges.node | `Product` | | product node, please see `Product` definition below

<a name="product" />

## Query Product
Query a single `Product` with `relay_id`

```
query product($id: ID!) {
  node(id: $id) {
    ... on Product {
      id
      name
      brand
      consumerCategories {
        categoryId
        path
      }
      coverImage
      skus {
        id
        price
        color
        size
        shippingPrice
        shippingTime
      }
    }
  }
}
```

`Product` fields:

field name | type | required | description
--- | --- | --- | ---
id | ID | True | product `relay_id`
brand | String | True | brand name
coverImage | `Image` | True | cover image object
coverVideo | `Video` | | video object
consumerCategories | `[ConsumerCategory]` | True | product categories
description | String | True | product description
enabled | Boolean | True | whether this product could be sell
extraImages | `[Image]` | | product images list object
features | `[String]` | | product features, there are at most `5` features
name | String | True | product name
inventory | Int | True | total count could to sell
skus | `[Sku]` | True | Sku list
targetUserType | `TargetUserType` | True | product is fit for
whiteBackgroundImage | `Image` | True | white background image object

<a name="sku" />

## Query Sku
Query a singe `Sku` with `relay_id`

```
query sku($id: ID!) {
  node(id: $id) {
    ... on Sku {
      id
      color
      size
      price
      product {
        id
        name
      }
    }
  }
}
```

`Sku` fields:

field name | type | required | description
--- | --- | --- | ---
id | `ID` | True | `Sku` `relay_id`
color | String | True | e.g., red / blue
coverImage | `Image` | True | cover image for this Sku
enabled | Boolean | True | this Sku whether could be sell
height | String | | default unit is `inch`
inventory | Int | True | total count could be sell
length | String | | default unit is `inch`
price | Float | True | currency unit is `USD`
product | `Product` | True | retrieve Sku's product
shippingPrice | Float | True | Shipping price for this Sku, product all skus' shipping price are same
shippingTime | String | True | Shipping days range, e.g., 3-10 days, product all skus' shipping time are same
size | String | | default unit is `inch`
weight | String | | default unit is `Pound`
width | String | | default unit is `inch`

## ConsumerCategory

Product maybe belongs to multiple categories, category fields:

field name | type | required | description
--- | --- | --- | ---
categoryId | String | True | Shoppo own category ID string, e.g., `5010101`
path | String | True | category path, e.g., `Beauty > Makeup & Beauty > Fragrance > Aromatherapy`

 ## TargetUserType Enum

value | description
--- | ---
ALL | for all
MEN | male only
WOMEN | female only
UNKNOWN | not sure
