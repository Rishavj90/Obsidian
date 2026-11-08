[setup drizzle with neon](https://orm.drizzle.team/docs/get-started/neon-new)

# Schema
[schema](https://orm.drizzle.team/docs/sql-schema-declaration)
```ts
import { integer, pgTable, varchar } from "drizzle-orm/pg-core";

export const usersTable = pgTable("users", {
  id: integer().primaryKey().generatedAlwaysAsIdentity(),
  name: varchar().notNull(),
  age: integer().notNull(),
  email: varchar().notNull().unique(),
});

```

- `pg-core` for PostgreSQL
- `uuid` : universal unique identifiers in pgSQL, alphanumeric string

# Migration
- convert drizzle-orm code => actual pgSQL
```shell
npx drizzle-kit generate
# generate pgSQL

npx drizzle-kit drop
#deletes all files
```
[[migration]]
