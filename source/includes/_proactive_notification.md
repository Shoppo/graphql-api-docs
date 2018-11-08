

# Proactive Notification

In order to notify you of our product and sku changes in a timely manner, we need you to provide the following interface for us to call.

For each interface, you need to provide a url to accept the corresponding parameters and return the correct result. For the correct result, you need to return a response code of 200. For other incorrect results, return a non-200 response code.

## Create Product

### post params

```json
{
    "identifier": "",
    "brand": "",
    "tags": "",
    "name": "",
    "inventory": "",
    "target_user_type": "",
    "enabled": "",
    "category_id": "",
    "consumer_category": [
        {
            "id": "",
            "original_id": "",
            "category_id": "",
            "path": ""
        }
    ],
    "cover_image": {
        "id": "",
        "original_id": "",
        "checksum": "",
        "original_width": "",
        "original_height": "",
        "original_url": "",
        "main_color": "",
        "type": "",
        "versions": "",
        "schema": ""
    },
    "white_background_image": {
        "id": "",
        "original_id": "",
        "checksum": "",
        "original_width": "",
        "original_height": "",
        "original_url": "",
        "main_color": "",
        "type": "",
        "versions": "",
        "schema": ""
    },
    "cover_video": {
        "id": "",
        "original_id": "",
        "checksum": "",
        "raw_path": "",
        "hls_path": "",
        "cover_image": {
            "id": "",
            "original_id": "",
            "checksum": "",
            "original_width": "",
            "original_height": "",
            "original_url": "",
            "main_color": "",
            "type": "",
            "versions": "",
            "schema": ""
        }
    },
    "extra_images": [
        {
            "id": "",
            "original_id": "",
            "checksum": "",
            "original_width": "",
            "original_height": "",
            "original_url": "",
            "main_color": "",
            "type": "",
            "versions": "",
            "schema": ""
        }
    ],
    "merchant": {
        "id": "",
        "name": ""
    }
}
```

Name | Type | Required | Description
--- | --- | --- | ---
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


## Update Product

### post params

```json
{
    "product_id": "",
    "identifier": "",
    "brand": "",
    "tags": "",
    "name": "",
    "inventory": "",
    "target_user_type": "",
    "enabled": "",
    "category_id": "",
    "consumer_category": [
        {
            "id": "",
            "original_id": "",
            "category_id": "",
            "path": ""
        }
    ],
    "cover_image": {
        "id": "",
        "original_id": "",
        "checksum": "",
        "original_width": "",
        "original_height": "",
        "original_url": "",
        "main_color": "",
        "type": "",
        "versions": "",
        "schema": ""
    },
    "white_background_image": {
        "id": "",
        "original_id": "",
        "checksum": "",
        "original_width": "",
        "original_height": "",
        "original_url": "",
        "main_color": "",
        "type": "",
        "versions": "",
        "schema": ""
    },
    "cover_video": {
        "id": "",
        "original_id": "",
        "checksum": "",
        "raw_path": "",
        "hls_path": "",
        "cover_image": {
            "id": "",
            "original_id": "",
            "checksum": "",
            "original_width": "",
            "original_height": "",
            "original_url": "",
            "main_color": "",
            "type": "",
            "versions": "",
            "schema": ""
        }
    },
    "extra_images": [
        {
            "id": "",
            "original_id": "",
            "checksum": "",
            "original_width": "",
            "original_height": "",
            "original_url": "",
            "main_color": "",
            "type": "",
            "versions": "",
            "schema": ""
        }
    ],
    "merchant": {
        "id": "",
        "name": ""
    }
}
```

Name | Type | Required | Description
--- | --- | --- | ---
product_id | String | True | **product** `relay_id`
brand | String | False | brand name
coverImage | [Image](#image) | False | cover image object
coverVideo | [Video](#video) | False | video object
consumerCategories | [[ConsumerCategory](#consumer-category)] | False | product categories
description | String | False | product description
enabled | Boolean | False | whether this product could be sell
extraImages | [[Image](#image)] | False | product images list object
features | [String] | False | product features, there are at most 5 features
name | String | False | product name
inventory | Int | False | total count could to sell
skus | [[Sku](#sku-node)] | False | Sku list
targetUserType | [TargetUserType](#target-user-type) | False | product is fit for
whiteBackgroundImage | [Image](#image) | False | white background image object

## Create Sku

### post params

```json
{
    "identifier": "",
    "description": "",
    "color": "",
    "size": "",
    "inventory": "",
    "price": "",
    "shipping_price": "",
    "msrp": "",
    "shipping_time": "",
    "length": "",
    "width": "",
    "height": "",
    "weight": "",
    "cover_image": {
        "id": "",
        "original_id": "",
        "checksum": "",
        "original_width": "",
        "original_height": "",
        "original_url": "",
        "main_color": "",
        "type": "",
        "versions": "",
        "schema": ""
    },
    "enabled": "",
    "global_shipping_config": "",
    "merchant": ""
}
```

Name | Type | Required | Description
--- | --- | --- | ---
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


## Update Sku

### post params

```json
{
    "sku_id": "",
    "identifier": "",
    "description": "",
    "color": "",
    "size": "",
    "inventory": "",
    "price": "",
    "shipping_price": "",
    "msrp": "",
    "shipping_time": "",
    "length": "",
    "width": "",
    "height": "",
    "weight": "",
    "cover_image": {
        "id": "",
        "original_id": "",
        "checksum": "",
        "original_width": "",
        "original_height": "",
        "original_url": "",
        "main_color": "",
        "type": "",
        "versions": "",
        "schema": ""
    },
    "enabled": "",
    "global_shipping_config": "",
    "merchant": ""
}
```

Name | Type | Required | Description
--- | --- | --- | ---
sku_id | ID | True | **Sku** `relay_id`
color | String | False | e.g., red / blue
coverImage | [Image](#image) | False | cover image for this Sku
enabled | Boolean | False | this Sku whether could be sell
height | String | False | default unit is **inch**
inventory | Int | False | total count could be sell
length | String | False | default unit is **inch**
msrp | Float | False | MSRP
price | Float | False | currency unit is **USD**
product | [Product](#product-node) | False | retrieve Sku's product
shippingConfig | [CustomerShippingConfig](#customer-shipping-config) | False | specialized shipping price and shipping time for multiple countries
shippingPrice | Float | False | Shipping price for this Sku, product all skus' shipping price are same
shippingTime | String | False | Shipping days range, e.g., 3-10 days, product all skus' shipping time are same
size | String | False | default unit is **inch**
weight | String | False | default unit is **Pound**
width | String | False | default unit is **inch**
