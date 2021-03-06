# postgraphile-upsert-plugin

Add postgres `upsert` mutations to [postgraphile](https://www.graphile.org/postgraphile).

[![JavaScript Style Guide](https://img.shields.io/badge/code_style-prettier-brightgreen.svg)](https://prettier.io/) ![npm](https://img.shields.io/npm/v/@lukeramsden/postgraphile-upsert-plugin)

## Getting Started

### Install

```bash
yarn add @lukeramsden/postgraphile-upsert-plugin
```

### CLI

```bash
postgraphile --append-plugins @lukeramsden/postgraphile-upsert-plugin:PgMutationUpsertPlugin
```

See [here](https://www.graphile.org/postgraphile/extending/#loading-additional-plugins) for
more information about loading plugins with PostGraphile.

### Library

```js
const express = require("express");
const { postgraphile } = require("postgraphile");
const {
  PgMutationUpsertPlugin
} = require("@lukeramsden/postgraphile-upsert-plugin");

const app = express();

app.use(
  postgraphile(pgConfig, schema, {
    appendPlugins: [PgMutationUpsertPlugin]
  })
);

app.listen(5000);
```

## Usage

This plugin creates an addition operation to `upsert<Table>` using a `where` clause, which is any unique constraint on the table. Supports multi-column unique indexes.

## Example

```sql
create table bikes (
  id serial PRIMARY KEY,
  "serialNumber" varchar UNIQUE NOT NULL,
  weight real,
  make varchar,
  model varchar
)
```

An upsert would look like this:

```graphql
mutation {
  upsertBike(
    where: { serialNumber: "abc123" }
    input: {
      bike: {
        serialNumber: "abc123"
        weight: 25.6
        make: "kona"
        model: "cool-ie deluxe"
      }
    }
  ) {
    clientMutationId
  }
}
```

## Credits

- This is a fork of the "upsert with where" [postgraphile-upsert-plugin](https://github.com/jashmenn/postgraphile-upsert-plugin)
- Which itself is a fork of the [TypeScript postgraphile-upsert-plugin](https://github.com/cdaringe/postgraphile-upsert)
- Which itself is a fork of [the original upsert plugin](https://github.com/einarjegorov/graphile-upsert-plugin/blob/master/index.js)
