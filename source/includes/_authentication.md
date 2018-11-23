# Authentication

You need to pass the `accesstoken` and `viewerid` fields in request header to pass the authentication.In addition, you need to set the Content-type to application/json.

- **Content-type**: application/json
- **accesstoken**: `your_accesstoken`
- **viewerid**: `your_viewerid`

<aside class="notice">
You must replace <code>your accesstoke</code> and <code>your viewerid</code> with your personal API key.
</aside>

## How to use

```graphql
query product($id: ID!) {
  node(id: $id) {
    ... on Product {
      id
      name
    }
  }
}
```

You can use query object like shown on the right in either **GraphiQL** playground or with **cURL**.

**Sku** is what our customers really see in our app. It contains specifications of the stuff(*ex. length, color*). Basic info of the stuff are in **Product** objects. A product can have **many** skus.

<aside class="notice">
If you use it in <b>GraphiQL</b>, you need to add <code>test_access_token</code> and <code>test_viewer_id</code> fields to the variables to pass the authentication because it is not convenient to pass them in the request header.
</aside>

To use **cURL**, you need a valid GraphQL request body. The format is: `{"query":"", "variables":{}, "operationName":""}`.

An example using Product Feed Query Object is shown below:

`curl -X POST -H "Content-Type: application/json" -H "accesstoken: your_accesstoken" -H "viewerid: your_viewerid" --data '{"query":"query product($id: ID!) {\n  node(id: $id) {\n    ... on Product {\n      id\n      name\n    }\n  }\n}\n","variables":{"id": "leZwJZoYvBfy"},"operationName":"Product"}' https://graphql-dev.shoppo.com/api/graphql/partner`
