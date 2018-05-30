## Create Order

Create order with Sku items and shipping address.

* When created, order status is unpaid.
* When user paid the order, call another request to set the order as paid.
* When user cancel the order before paid, call another request to cancel the order.

Create Order Mutation:
```graphql
mutation createOrder(
  $shippingAddress: ShippingAddressInput!,
  orderItems: [OrderItemInput!]!,
) {
  createOrder(
    shippingAddress: $shippingAddress,
    orderItems: $orderItems,
    ){
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
```

Response order structure details please see [Query Order](#queryOrder)

Fields:

Name | Type | Required | Description
--- | --- | --- | ---
shippingAddress | [ShippingAddressInput](#shippingAddress) | True | address object
orderItems | [[OrderItemInput](#orderItemInput)!]! | True | order items contains Sku ID and quantity

OrderItemInput Fields:
<a name="orderItemInput" />

Name | Type | Required | Description
--- | --- | --- | ---
skuId | ID | True | Sku relay id
quantity | Int | True | buy Sku quantity

## Query Order
<a name="queryOrder" />

Query order with relay id

```graphql
query order($id: ID!) {
  node(id: $id) {
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
```

Order fields:

Name | Type | Required | Description
--- | --- | --- | ---
discount_amount | Float | True | discount amount
id | ID | True | order relay id
order_items | [[OrderItem](#queryOrderItem)] | True | order item list
order_subtotal | Float | True | sum of products amount and shipping fee
order_total | Float | True | customer paid amount
shipping_address | [shippingAddress](#shippingAddress) | True | address object
shipping_cost | Float | True | total shipping fee amount
status | [OrderStatus](#orderStatus) | True | order enums status
time_created | Int | True | unix timestamp for order created at
time_payment_processed | Int | | unix timestamp for order paid at

## Query Order Item
<a name="queryOrderItem" />

```graphql
query orderItem($id: ID!) {
  node(id: $id) {
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
```

Order item fields:

Name | Type | Required | Description
--- | --- | --- | ---
id | ID | True | order item relay id
is_refunded | Boolean | | the order item whether is refunded or not
order | [Order](#queryOrder) | True | order item's order object
product | [Product](./product.md#product) | True | product snapshot
`product.id` | ID | True | product snapshot relay id
`product.originalId` | ID | True | product relay id, if you want retrieve product, use `originalId` other than `id`
quantity | Int | True | number for buy this Sku
refunded_amount | Float | | if order item is refunded, the amount will be return
shipping_refunded | Boolean | | if refunded, the refunded amount whether contains shipping fee or not
sku | [Sku](./product.md#sku) | True | sku snapshot
`sku.id` | ID | True | Sku snapshot relay id
`sku.originalId` | ID | True | Sku relay id, if you want retrieve Sky, use `originalId` other than `id`
shipping_provider | String | | Courier name for shipping package
status | [OrderItemStatus](#orderItemStatus) | True | order item status enum
tracking_number | String | | tracking number for the package
time_refunded | Int | | if refunded, unix timestamp for refund created at

## Set Order as Paid (TODO)

## Cancel Order Before Paid (TODO)

## Refund Order Item After Paid (TODO)

## Shipping Address
<a name="shippingAddress"></a>
The address for packages shipped to customer

Fields:

Name | Type | Required | Description
--- | --- | --- | ---
recipientFirstName | String | True recipient first name
recipientLastName | String | True | recipient last name
streetAddress1 | String | True | Address 1
streetAddress2 | String | True | Address 2
city | String | True | city name
state | String | True | state name
country | String | True | country name, e.g. `United States`

### Order Status
<a name="orderStatus" />
Order status enums

Value | Description
--- | ---
OPEN | order created
PAID | order has been paid
IN_PROGRESS | packages are being prepared in warehouse
DELIVERED | packages are sent to couriers
CLOSED | customer cancelled the order
EXPIRED | order expired and could not pay for this order

### OrderItem Status
<a name="orderItemStatus" />
OrderItem status enums

Value | Description
--- | ---
OPEN | order has not been paid
PAID | order has been paid
IN_FULFILLMENT | order item package is being prepared in warehouse
SHIPPED | the package is on the way to deliver to customer
DELIVERED | customer confirmed he received the package
CANCELLED | order item is cancelled

## Shipping Package (TODO)
