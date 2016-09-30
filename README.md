This is a CycleJS driver for talking to a GraphQL endpoint.

It uses [Apollo Client](http://docs.apollostack.com/apollo-client/core.html) underneath.

* the graphql endpoint defaults to `/graphql`, but it can be changed by passing the `endpoint='/something'` option to `makeGraphQLDriver`;
* `withCredentials` is enabled by default (and there's no way to change it);
* to send custom headers with the GraphQL requests, it is possible to either
  * specify the headers through the `headers={authorization: 'token xyz'}` option to `makeGraphQLDriver`; or
  * emit an object with a key `headers` (example: `{headers: {authorization: 'token xyz'}}`) as an event in the stream that the driver is consuming.


### Install


### Use


### Roadmap

* Add TypeScript typings
* Make diversity compliant (currently uses xstream exclusively)
* Use docs
