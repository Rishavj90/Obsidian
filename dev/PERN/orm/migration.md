# **What is a Migration?**

A migration is a **versioned record of database schema changes**. It's a file that says "go from state A to state B."

Instead of manually running SQL on your database, migrations let you:

- Version control your database changes (git history)
- Safely rollback changes
- Deploy consistently across environments (dev → prod)

---

**Without Migrations (❌ Bad):**

```typescript
// You manually run SQL in the database:
CREATE TABLE users (id INT PRIMARY KEY, name VARCHAR);

// Later you need to add a column:
ALTER TABLE users ADD COLUMN email VARCHAR;

// Problem: No version history, other developers don't know these changes exist
```

**With Migrations (✅ Good):**

```
migrations/
├── 0001_create_users_table.sql
└── 0002_add_email_column.sql

// Each file is a timestamped, tracked version
// Everyone runs migrations in order: both changes apply automatically
```

---

**How Drizzle Migrations Work**

**Step 1: Define your schema**

```typescript
// src/schema.ts
import { pgTable, serial, text } from 'drizzle-orm/pg-core';

export const users = pgTable('users', {
  id: serial('id').primaryKey(),
  name: text('name'),
});
```

**Step 2: Generate migration**

```bash
npm run drizzle-kit generate
```

Drizzle compares your schema to the database and creates a migration file:

```sql
-- migrations/0001_create_users.sql
CREATE TABLE "users" (
  "id" SERIAL PRIMARY KEY,
  "name" TEXT
);
```

**Step 3: Run migration**

```bash
npm run drizzle-kit migrate
```

This executes the SQL file against your database.

---

**Later: Add a Column**

```typescript
// Update schema
export const users = pgTable('users', {
  id: serial('id').primaryKey(),
  name: text('name'),
  email: text('email'), // NEW
});
```

```bash
npm run drizzle-kit generate
```

Creates a new migration:

```sql
-- migrations/0002_add_email.sql
ALTER TABLE "users" ADD COLUMN "email" TEXT;
```

```bash
npm run drizzle-kit migrate
```

---

**Key Benefits**

|Without Migrations|With Migrations|
|---|---|
|Manual SQL|Automated|
|No history|Versioned (git)|
|Hard to rollback|Easy rollback|
|Team sync issues|Everyone in sync|
|Production errors|Tested locally first|

---

**Common Drizzle Commands**

```bash
# Generate migration files from schema changes
npm run drizzle-kit generate

# Apply pending migrations to database
npm run drizzle-kit migrate

# Drop all tables (⚠️ use carefully)
npm run drizzle-kit drop

# View migration status
npm run drizzle-kit check
```

---

**Migration File Structure**

```typescript
// drizzle.config.ts
export default {
  schema: './src/schema.ts',          // Where your schema is defined
  out: './drizzle',                   // Where migrations are saved
  driver: 'pg',                       // PostgreSQL
  dbCredentials: {
    connectionString: process.env.DATABASE_URL,
  },
};
```

---

# **Workflow Summary**

1. Edit `schema.ts` → define table/columns
2. Run `drizzle-kit generate` → creates migration file
3. Run `drizzle-kit migrate` → applies to database
4. Commit migration files to git
5. Other devs/production run migrations automatically

**Bottom line:** Migrations are the single source of truth for your database structure. Always generate → test locally → commit → deploy.