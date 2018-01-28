# TypeScript promiseCache

The `promiseCache` dectorator and service allow an easy ability to apply caching for a method returning an async return.

Caching promises can be useful when similar components are calling the same endpoint which can be common in larger forms or dropdown lists.

## Overview

- `promiseCache.decorator.ts` - The `@promiseCache()` dectorator allows caching of the entire method or based on exact parameters.
- `promiseCache.service.ts` - Stores a cache dictionary that allows clearing of data. This could be extended easily to cache across browser sessions if needed.

I will take pull requests on this repo. Mostly putting the code out there for others and for myself since I use it in a few locations.

## Examples

### Without Paramters

The easiest example is caching a basic `getItems` request.

```TypeScript
@PromiseCache()
async getItems(): Promise<Item[]> {
  let res = await this.http.get<Item>('/api/items')
    .toPromise();
  return res.map(i => new Item().from(i)); // .from is a dto mapper
}
```

### With Parameters

Extending the example above with a basic search filter for a `term` paramters.

```TypeScript
@PromiseCache()
async getItems(@CacheParam term: string): Promise<Item[]> {
  let res = await this.http.get<Item>('/api/items', {
      params: {
          term: term
      }
  }).toPromise();
  return res.map(i => new Item().from(i)); // .from is a dto mapper
}
```