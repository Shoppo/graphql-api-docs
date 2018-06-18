## Ticket List

```graphql
query ticketList(
    $first: Int,
    $last: Int,
    $after: String,
    $before: String,
    $filters: TicketFilterInput,
) {
  ticketList(
    first: $first,
    last: $last,
    after: $after,
    before: $before,
    filters: $filters,
  ) {
    id
    edges {
      node {
        id
        status
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
filters | [TicketFilterInput](#ticketFilterInput) | | filter parameters

`TicketFilterInput` fields:

Name | Type | Required | Description
--- | --- | --- | ---
timeCreatedRange | [Int, Int] | | filter by created time range, time format is `unix timestamp`

Response ticket list fields:

field name | type | required | description
--- | --- | --- | ---
length | Int | True | total matches count
edges | List | True | ticket node list
edges.node | [Ticket](#ticketNode) | | ticket node, please see [Ticket](#ticketNode) definition below

## Shoppo Ticket Workflow

![GraphiQLProduct](./imgs/ticket-flow.png)

## Create Ticket

Create ticket with content.

* When created, the ticket status is new.
* When customer reply the ticket, call another request to set the ticket status.

Create Ticket Mutation:
```graphql
mutation createTicket(
  $content: String!,
) {
  createTicket(
    content: $content,
    ){
      ticket {
        id
        status
        contents
      }
  }
}
```

Response ticket structure details, please see [Query Ticket](#queryTicket)

Variables:

Name | Type | Required | Description
--- | --- | --- | ---
content | String | True | ticket content


<a name="queryTicket" />
## Query Ticket

Query ticket with relay id

```graphql
query ticket($id: ID!) {
  node(id: $id) {
    id
    status
    content
  }
}
```

<a name="ticketNode" />

`Ticket` fields:

Name | Type | Required | Description
--- | --- | --- | ---
id | ID | True | ticket relay id
status | [TicketStatus](#ticketStatus) | True | ticket enums status
contents | List | True | ticket content list


### Ticket Status
Ticket status enums

Value | 
--- |
NEW | 
OPEN | 
PENDING |
HOLD |
SOLVED |
CLOSED | 

## Ticket Reply Callback

We need you to provide a url to accept the post method for the shoppo call to reply ticket.

Post Variables:

Name | Type | Required | Description
--- | --- | --- | ---
id | ID | True | ticket relay id
content | String | True | ticket relay content

## Relay Ticket

Users can reply to the ticket that was returned.

```graphql
mutation relayTicket(
  $id: ID!,
  $content: String!,
) {
  relayTicket(
    id: $id,
    content: $content,
    ){
      ticket {
        id
        status
        contents
      }
  }
}
```

Response ticket structure details, please see [Query Ticket](#queryTicket)

Variables:

Name | Type | Required | Description
--- | --- | --- | ---
id | ID | True | ticket relay id
contents | List | True | ticket content list
