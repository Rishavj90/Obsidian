## The Database Question: Where Does Your Data Live?

### Option A: Local PostgreSQL (Recommended for Learning)

### Option B: Managed PostgreSQL (Like MongoDB Atlas)

If you want cloud hosting:

- **Neon** (free tier, great for learning)
- **Railway** (simple setup)
- **AWS RDS**

---
# Neon
[neon docs](https://neon.com/docs/guides/express)
```ts
import express from "express";

import 'dotenv/config';

import { neon } from "@neondatabase/serverless";

  

const app = express()

const port:number = parseInt(process.env.PORT || "8000", 10)

const sql = neon(process.env.DATABASE_URL!);

  
  
  

app.listen(port, async()=>{

try {

await sql`SELECT version()`;

console.log(`Database connected`)

console.log(`server is running on ${port}`)

} catch (error) {

console.error('Failed to connect to the database', error);

}

})
```