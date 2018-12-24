# Product

You can query the product list and product category through API.

<aside class="notice">
Since some of our previous categories did not distinguish between men and women, the two categories of men and women shared the same category id. In order to distinguish, we did the following processing, adding 0 and 1 after the original category id, 0 at the end of the original category represents male or boy, 1 represents female or girl. So if the category id is 7 digits, then the original category has not changed. If the category id is 8 digits, the last digit is the marker used to distinguish between men and women.
</aside>

## Query Single Product

Query a single **Product** with `relay_id`

```graphql
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
      coverImage {
        id
      }
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

<span id="product-node"></span>

### Product Fields

Name | Type | Required | Description
--- | --- | --- | ---
id | ID | True | **product** `relay_id`
brand | String | True | brand name
coverImage | [Image](#image) | True | cover image object
coverVideo | [Video](#video) | False | video object
consumerCategories | [[ConsumerCategory](#consumer-category)] | True | product categories
description | String | True | product description
enabled | Boolean | True | whether this product could be sell
extraImages | [[Image](#image)] | False | product images list object
features | [String] | False | product features, there are at most 5 features
name | String | True | product name
inventory | Int | True | total count could to sell
skus | [[Sku](#sku-node)] | True | Sku list
targetUserType | [TargetUserType](#target-user-type) | True | product is fit for
whiteBackgroundImage | [Image](#image) | True | white background image object

## Query Single Sku

Query a singe **Sku** with `relay_id`

```graphql
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

<span id="sku-node"></span>

### Sku Fields

Name | Type | Required | Description
--- | --- | --- | ---
id | ID | True | **Sku** `relay_id`
color | String | True | e.g., red / blue
coverImage | [Image](#image) | True | cover image for this Sku
enabled | Boolean | True | this Sku whether could be sell
height | String | False | default unit is **inch**
inventory | Int | True | total count could be sell
length | String | False | default unit is **inch**
msrp | Float | False | MSRP
price | Float | True | currency unit is **USD**
product | [Product](#product-node) | True | retrieve Sku's product
shippingConfig | [CustomerShippingConfig](#customer-shipping-config) | True | specialized shipping price and shipping time for multiple countries
shippingPrice | Float | True | Shipping price for this Sku, product all skus' shipping price are same
shippingTime | String | True | Shipping days range, e.g., 3-10 days, product all skus' shipping time are same
size | String | False | default unit is **inch**
weight | String | False | default unit is **Pound**
width | String | False | default unit is **inch**

## Product List

Query products with pagination(using **Relay Connection**, check [End-of-list, counts, and Connections](https://graphql.org/learn/pagination/#end-of-list-counts-and-connections) for details)

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

### Variables

Name | Type | Required | Description
--- | --- | --- | ---
first | Int | False | limit size
last | Int | False | limit size
after | String | False | offset cursor
before | String | False | offset cursor
filters | [ProductFilterInput](#product-filter-input) | False | filters

<span id="product-filter-input"></span>

### ProductFilterInput Fields

Name | Type | Required | Description
--- | --- | --- | ---
timeCreateRange | [Int, Int] | False | filter by created time range, time format is **unix timestamp**
timeUpdatedRange | [Int, Int] | False | filter by updated time range, time format is **unix timestamp**
productEnabled | Boolean | False | is product enabled
categoryId | String | False | category id

### Response Product List Fields

Name | Type | Required | Description
--- | --- | --- | ---
length | Int | True | total matches count
edges | List | True | node list
edges.node | [Product](#product-node) | | product node, please see [Product Fields](#product-node) definition below

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

Response node type is [ConsumerProductCategory](#consumer-category)

<span id="consumer-category"></span>

### Consumer Category

Product maybe belongs to multiple categories, category fields:

Name | Type | Required | Description
--- | --- | --- | ---
id | ID | True | relay id
categoryId | String | True | Shoppo own category ID string, e.g., `5010101`
path | String | True | category path, e.g., `Beauty > Makeup & Beauty > Fragrance > Aromatherapy`

<span id="target-user-type"></span>

### TargetUserType Enum

Value | Description
--- | ---
ALL | for all
MEN | male only
WOMEN | female only
UNKNOWN | not sure

<span id="customer-shipping-config"></span>

### Customer Shipping Config

Name | Type | Required | Description
--- | --- | --- | ---
countryCode | [CountryCode](#country-code) | True | country code enum
shippingPrice | Float | True | special shipping price for this country
shippingTime | String | True | special shipping time for this country

<span id="country-code"></span>

### Country Code Enum

Value | Country
--- | ---
US | United States
CN | China
IN | India
MY | Malaysia
SG | Singapore
KE | Kenya
