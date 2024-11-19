# aws-sdk repro

## How to reproduce

```bash
npm install
npm run check
```

## Description

The issue was introduced in `v3.691.0` and remains in the latest version (`v3.693.0` as of 2024-11-19)
Downgrading to `@aws-sdk/client-dynamodb@3.687.0` and `@aws-sdk/lib-dynamodb@3.689.0` will resolve it.
These versions are the latest versions released prior to `v3.691.0`.

The easiest way to get a list of the affected files is to set `skipLibCheck` to `false` in `tsconfig.json`.
This way one only have to import from `@aws-sdk/lib-dynamodb` to trigger the error.

Note that setting `skipLibCheck` to `true` in `tsconfig.json`, will require actually using the affected code,
to trigger the error, e.g.:

```typescript
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DynamoDBDocumentClient, ScanCommand } from "@aws-sdk/lib-dynamodb";

const dyndbClient = new DynamoDBClient({});

export const dyndbDocClient = DynamoDBDocumentClient.from(dyndbClient);

dyndbDocClient.send(new ScanCommand({ TableName: "my-table" }));
```
