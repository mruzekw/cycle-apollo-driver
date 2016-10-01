This is a CycleJS driver for talking to a GraphQL endpoint.

It uses [Apollo Client](http://docs.apollostack.com/apollo-client/core.html) underneath.

* the graphql endpoint defaults to `/graphql`, but it can be changed by passing the `endpoint='/something'` option to `makeGraphQLDriver`;
* `withCredentials` is enabled by default (and there's no way to change it);
* to send custom headers with the GraphQL requests, it is possible to either
  * specify the headers through the `headers={authorization: 'token xyz'}` option to `makeGraphQLDriver`; or
  * emit an object with a key `headers` (example: `{headers: {authorization: 'token xyz'}}`) as an event in the stream that the driver is consuming.


### Install

```
npm install --save cycle-apollo-driver
```


### Use

```javascript
function app({GraphQL}) {

  let response$ = GraphQL
    .compose(flattenConcurrently)
    .map(({data}) => data);

  response$.addListener({
    next: s => { console.log(s) },
    error: err => console.error(err),
    complete: () => {},
  });

  return {
    GraphQL: xs.fromArray([{
      query: 'fetchPost',
      variables: {
        id: 'randomid'
      }
    }, {
      mutation: 'createPost',
      variables: {
        imageUrl: 'an item',
        desc: 'this is an item'
      }
    }])
  };

}

const GraphQLConfig = {
  endpoint: 'https://api.graph.cool/simple/v1/citfkupnt0oxk0134czs10yid',
  templates: {
    fetchPost: gql`
query fetchPost($id: ID!) {
  Post(id: $id) {
    imageUrl
    description
  }
}
    `,
    fetchAll: gql`
query {
  allPosts {
    imageUrl
    description
  }
}
    `,
    createPost: gql`
mutation createPost($imageUrl: String!, $desc: String!) {
  createPost(imageUrl: $imageUrl, description: $desc) {
    imageUrl
    description
  }
}
    `
  }
};

Cycle.run(app, {
  GraphQL: makeGraphQLDriver(GraphQLConfig)
});
```


### Roadmap

* Add TypeScript typings
* Make diversity compliant (currently uses xstream exclusively)
