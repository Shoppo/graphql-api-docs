# Order

## Shoppo Order Workflow

![](./images/order-flow.png)

## Order List

```graphql
query orderList(
    $first: Int,
    $last: Int,
    $after: String,
    $before: String,
    $filters: OrderFilterInput,
) {
  orderList(
    first: $first,
    last: $last,
    after: $after,
    before: $before,
    filters: $filters,
  ) {
    length
    edges {
      node {
        id
        status
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
filters | [OrderFilterInput](#order-filter-input) | False | filter parameters

<span id="order-filter-input"></span>

### OrderFilterInput Fields

Name | Type | Required | Description
--- | --- | --- | ---
order_id | String | False | The shoppo order id
partner_order_code | String | False | The partner order code
sku_id | String | False | The sku_id in order
order_csv_upload_record_id | String | False | The order csv upload record id
time_payment_processed_range | [Int, Int] | False | Time payment range
status_in | [[OrderStatus](#order-status)] | Fasle | order status list

### Response Order List Fields

field name | type | required | description
--- | --- | --- | ---
length | Int | True | total matches count
edges | List | True | order node list
edges.node | [Order](#order-node) | False | order node, please see [Order Node](#order-node) definition below



## Sync Order Status Workflow


![](./images/order-list.png)


## Create Order

Create order with Sku items and shipping address.

* When created, order status is unpaid.
* When user paid the order, call another request to set the order as paid.
* When user cancel the order before paid, call another request to cancel the order.

```graphql
mutation createOrder(
  $shippingAddress: ShippingAddressInput!,
  $orderItems: [OrderItemInput!]!,
  $partnerOrderCode: String!,
  $trackingNumber: String
) {
  createOrder(
    shippingAddress: $shippingAddress,
    orderItems: $orderItems,
    partnerOrderCode: $partnerOrderCode,
    trackingNumber: $trackingNumber
    ){
      order {
        id
        status
        orderItems {
          id
          sku {
            originalId
            price
          }
          product {
            originalId
            name
          }
          quantity
        }
      }
  }
}
```

### Variables

Name | Type | Required | Description
--- | --- | --- | ---
shippingAddress | [ShippingAddressInput](#shipping-address) | True | address object
orderItems | [[OrderItemInput](#order-item-input)!]! | True | order items contains Sku ID and quantity
partnerOrderCode | String | True | partner code code
trackingNumber | String | False | partner tracking number

<span id="order-item-input"></span>

### OrderItemInput Fields

Name | Type | Required | Description
--- | --- | --- | ---
skuId | ID | True | Sku relay id
quantity | Int | True | buy Sku quantity

The response of order structure details, please see [Order Node](#Order-node)

## Query Order

Query order with `relay_id`

```graphql
query order($id: ID!) {
  node(id: $id) {
    ... on order {
      id
      orderItems {
        id
        sku {
          originalId
        }
        status
      }
      status
    }
  }
}
```

<span id="order-node"></span>

### Order Fields

Name | Type | Required | Description
--- | --- | --- | ---
discount_amount | Float | True | discount amount
id | ID | True | order relay id
order_items | [[OrderItem](#order-item-node)] | True | order item list
order_subtotal | Float | True | sum of products amount and shipping fee
order_total | Float | True | customer paid amount
shipping_address | [ShippingAddress](#shipping-address) | True | address object
shipping_cost | Float | True | total shipping fee amount
status | [OrderStatus](#order-status) | True | order enums status
refund_status | [OrderRefundStatus](#order-refund-status) | False |  order enums refund status
time_created | Int | True | unix timestamp for order created at
time_payment_processed | Int | False | unix timestamp for order paid at
tracking_number | String | False | order tracking number

## Query OrderItem

```graphql
query orderItem($id: ID!) {
  node(id: $id) {
    ... on orderItem {
      id
      sku {
        id
        originalId
        price
        shippingPrice
      }
      product {
        id
        originalId
        name
      }
      status
    }
  }
}
```

<span id="order-item-node"></span>

### Order Item Fields

Name | Type | Required | Description
--- | --- | --- | ---
id | ID | True | order item relay id
is_refunded | Boolean | False | the order item whether is refunded or not
order | [Order](#order-node) | True | order item's order object
product | [Product](#product-node) | True | product snapshot
`product.id` | ID | True | product snapshot relay id
`product.originalId` | ID | True | product relay id, if you want retrieve product, use `originalId` other than `id`
quantity | Int | True | number for buy this Sku
refundedAmount | Float | False | if order item is refunded, the amount will be return
shippingRefunded | Boolean | False | if refunded, the refunded amount whether contains shipping fee or not
sku | [Sku](#sku-node) | True | sku snapshot
`sku.id` | ID | True | Sku snapshot relay id
`sku.originalId` | ID | True | Sku relay id, if you want retrieve Sku, use `originalId` other than `id`
shippingPackage | [ShippingPackage](#shipping-package) | False | order item's shipping package
shippingProvider | String | False | Courier name for shipping package
status | [OrderItemStatus](#order-item-status) | True | order item status enum
trackingNumber | String | False | tracking number for the package
timeRefunded | Int | False | unix timestamp for refund created at if refunded

## Set Order as Paid

Before set order as paid, please check the order status and it should be `OPEN`.
When set order as paid successfully, the status will be `PAID`.

```graphql
mutation setOrderAsPaid($orderId: ID!, $chargeId: String!) {
  setOrderAsPaid(orderId: $orderId, chargeId: $chargeId) {
    order {
      id
      status
      timePaymentProcessed
      orderItems {
        id
        status
      }
    }
  }
}
```

### Variables

Name | Type | Required | Description
--- | --- | --- | ---
orderId | ID | True | order relay id to set paid
chargeId | String | True | Partner charge id for the order and should be unique

## Cancel OrderItem

If order is not paid, you don't need cancel order. Only when order item status is `PAID`, you could cancel order item directly, otherwise you need create customer service ticket.

```graphql
mutation cancelOrderItem($orderItemId: ID!) {
  cancelOrderItem(orderItemId: $orderItemId) {
    orderItem {
      id
      status
    }
  }
}
```

### Variables

Name | Type | Required | Description
--- | --- | --- | ---
orderItemId | ID | True | order item relay id to cancel

## Cancel Order

If your order has many order items and you need to cancel all items, you can use the cancel order interface as below. Same as cancel order item, only when all order items status of this order are `PAID`, you could cancel order directly, otherwise you need create customer service ticket.


```graphql
mutation cancelOrder($orderId: ID!) {
  cancelOrder(orderId: $orderId) {
    order {
      id
      status
    }
  }
}
```

### Variables

Name | Type | Required | Description
--- | --- | --- | ---
orderId | ID | True | order relay id to cancel

<span id="shipping-address"></span>

### Shipping Address Fields

The address for packages shipped to customer

Name | Type | Required | Description
--- | --- | --- | ---
recipientFirstName | String | True | recipient first name
recipientLastName | String | True | recipient last name
streetAddress1 | String | True | Address 1
streetAddress2 | String | True | Address 2
city | String | True | city name
state | String | True | state name
countryCode | [CountryCode](#country-code)| True | country code, e.g.  `US` for `United States`
zipcode | String | True | zip code
phoneNumber | String | True | recipient phone number

<span id="order-status"></span>

### Order Status Enums

Value | Description
--- | ---
OPEN | order created
PAID | order has been paid
IN_PROGRESS | packages are being prepared in warehouse
DELIVERED | packages are sent to couriers
CLOSED | customer cancelled the order
EXPIRED | order expired and could not pay for this order

<span id="order-refund-status"></span>

### Order Refund Status Enums

Value | Description
--- | ---
FULL_REFUNDED | all order items are refunded
PARTIAL_REFUNDED | part of order items are refunded
NOT_REFUNDED | none of order items are refunded

<span id="order-item-status"></span>

### OrderItem Status Enums

Value | Description
--- | ---
OPEN | order has not been paid
PAID | order has been paid
IN_FULFILLMENT | order item package is being prepared in warehouse
SHIPPED | the package is on the way to deliver to customer
DELIVERED | customer confirmed he received the package
CANCELLED | order item is cancelled

<span id="shipping-package"></span>

### Shipping Package Fields

Name | Type | Required | Description
--- | --- | --- | ---
id | ID | True | relay id
checkpoints | [[Checkpoint](#checkpoint)] | False | tracking info
courier | String | True | shipping provider name
isDelivered | Boolean | True | whether package is delivered
status | String | True | packpage current status
timeDelivered | Int | False | delivered at, **unix timestamp**
timeLanded  | Int | False | landed at, **unix timestamp**
timeShipped | Int | False | shipped at, **unix timestamp**
timeTracked | Int | False | tracked at, **unix timestamp**
trackingNumber | String | True | tracking number

<span id="checkpoint"></span>

### Checkpoint Fields:

Name | Type | Required | Description
--- | --- | --- | ---
courier | String | False | courier name
time | Int | False | checkpoint happened at, **unix timestamp**
message | String | False | event description
location | String | False | location

<span id="refund-reason-type"></span>

### Refund Reason Type Enums

Value | Description
--- | ---
CUSTOMER_CANCELED | customer canceled order
OUT_OF_STOCK | sku is out of stock
BAD_SHIPPING_ADDRESS | the shipping address is wrong
AGREEMENT_WITH_USER | negotiate with the user
FRAUDULENT_PURCHASE | fraudulent purchase
NOT_SHIPPED_TOO_LONG | not transported for too long
