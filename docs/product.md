## Product List

Query products with pagination(using `Relay Connection`, check [https://www.learnrelay.org/connections/cursors-pagination/](https://www.learnrelay.org/connections/cursors-pagination/) for details)

```graphql
query productList(
  $first: Int,
  $after: String,
  $before: String,
  $last: Int,
  $filters: ProductFilterInput,
) {
  productList(
    first: $first,
    after: $after,
    before: $before,
    last: $last,
    filters: $filters,
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

Variables:

Name | Type | Required | Description
--- | --- | --- | ---
first | Int | | limit size
last | Int | | limit size
after | String | | offset cursor
before | String | | offset cursor
filters | [ProductFilterInput](#productFilterInput) | | filters

Connection parameters, more details about collection pagination please visit [http://graphql.org/learn/pagination/](http://graphql.org/learn/pagination/)

<a name="productFilterInput" />

`ProductFilterInput` fields:

Name | Type | Required | Description
--- | --- | --- | ---
timeUpdatedRange | [Int, Int] | | filter by updated time range, time format is `unix timestamp`

Product list fields:

field name | type | required | description
--- | --- | --- | ---
length | Int | True | total matches count
edges | List | True | node list
edges.node | [Product](#productNode) | | product node, please see [Product](#productNode) definition below

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

<a name="productNode" />

`Product` fields:

field name | type | required | description
--- | --- | --- | ---
id | ID | True | product `relay_id`
brand | String | True | brand name
coverImage | [Image](./image_and_video.md#image) | True | cover image object
coverVideo | [Video](./image_and_video.md#video) | | video object
consumerCategories | [[ConsumerCategory](#consumerCategory)] | True | product categories
description | String | True | product description
enabled | Boolean | True | whether this product could be sell
extraImages | [[Image]](./image_and_video.md#image) | | product images list object
features | `[String]` | | product features, there are at most `5` features
name | String | True | product name
inventory | Int | True | total count could to sell
skus | [Sku](#skuNode) | True | Sku list
targetUserType | [TargetUserType](#targetUserType) | True | product is fit for
whiteBackgroundImage | [Image](./image_and_video.md#image) | True | white background image object

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

<a name="skuNode" />

`Sku` fields:

field name | type | required | description
--- | --- | --- | ---
id | `ID` | True | `Sku` `relay_id`
color | String | True | e.g., red / blue
coverImage | [Image](./image_and_video.md#image) | True | cover image for this Sku
enabled | Boolean | True | this Sku whether could be sell
height | String | | default unit is `inch`
inventory | Int | True | total count could be sell
length | String | | default unit is `inch`
msrp | Float | | MSRP
price | Float | True | currency unit is `USD`
product | [Product](#productNode) | True | retrieve Sku's product
shippingPrice | Float | True | Shipping price for this Sku, product all skus' shipping price are same
shippingTime | String | True | Shipping days range, e.g., 3-10 days, product all skus' shipping time are same
size | String | | default unit is `inch`
weight | String | | default unit is `Pound`
width | String | | default unit is `inch`

<a name="productCategories" />

## Product Category List

Query for product category list

```graphql
query productCategories(
  $first: Int,
  $after: String,
  ){
  productCategories(
    first: $first,
    after: $after,
  ) {
    length
    edges {
      node {
        id
        categoryId
        path
      }
    }
  }
}
```

Response `node` type is [ConsumerProductCategory](#consumerCategory)

<a name="consumerCategory" />

## ConsumerCategory

Product maybe belongs to multiple categories, category fields:

field name | type | required | description
--- | --- | --- | ---
id | ID | True | relay id
categoryId | String | True | Shoppo own category ID string, e.g., `5010101`
path | String | True | category path, e.g., `Beauty > Makeup & Beauty > Fragrance > Aromatherapy`

<a name="targetUserType" />

## TargetUserType Enum

value | description
--- | ---
ALL | for all
MEN | male only
WOMEN | female only
UNKNOWN | not sure
