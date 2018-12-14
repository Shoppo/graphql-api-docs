# Introduction

Welcome to the SHOPPO GraphQL API!

SHOPPO chose GraphQL for our API because it offers significantly more flexibility for our integrators. The ability to define precisely the data you want—and only the data you want—is a powerful advantage over the REST API endpoints. GraphQL lets you replace multiple REST requests with a single call to fetch the data you specify.

## Endpoint

* Dev: [https://graphql-dev.shoppo.com/api/graphql/partner](https://graphql-dev.shoppo.com/api/graphql/partner)
* Prod: [https://graphql.shoppo.com/api/graphql/partner](https://graphql.shoppo.com/api/graphql/partner)

To use API, you need to POST a GraphQL compatible request body to our API endpoint. And you will get a JSON response.

## Playground

You can explore our API with GraphiQL here:

* Dev: [https://graphql-dev.shoppo.com/console/partner](https://graphql-dev.shoppo.com/console/partner)

![](./images/graphiql_playground.png)

### How to use Playground as a API reference

With GraphiQL, you can see a **docs** menu in the top right(as shown below)

![](./images/graphiql_docs.png)

In the menu, you can see our GraphQL Schema. Basically, **RootQuery** for supported query objects, and **RootMutation** for supported mutations. And of course, you can search a specific schema in the search box. For example, you search **Product** and see what is a **Product**.

![product](./images/graphiql_product.png)

As shown in the screenshot, all supported field are listed with name and type and you can know if the field is required. If you click on the field name, you can get a simple description of the field.


## Using Java

These links may help:

* [apollo-android](https://github.com/apollographql/apollo-android)
* [graphql_java_gen](https://github.com/Shopify/graphql_java_gen)
* [graphql-java](https://github.com/graphql-java/graphql-java)

<style>
img[alt=product] {
    width: 50%
}
</style>
