## The Actual Setup (Step by Step)

Let me give you the exact commands and explain what's happening:

### Step 1: Create Your Project

```bash
mkdir twitter-clone
cd twitter-clone
mkdir server
cd server
npm init -y
```

### Step 2: Install TypeScript Tooling

```bash
npm install express cors dotenv
npm install --save-dev typescript @types/express @types/node
```

**What each does:**
- `@types/*`: TypeScript definitions so TS knows what Express/Node methods are available

### Step 3: Configure TypeScript

```bash
tsc --init
```

This creates `tsconfig.json`. **Open it and find these lines, uncomment them:**

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "resolveJsonModule": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

**What this means:**

- Compile TypeScript to JavaScript that runs on Node
- `strict: true` = catch more errors (good for learning)
- `outDir` = where compiled JS goes

### Step 4: Create Your Startup Script

Open `package.json` and replace the `scripts` section:

```json
"scripts": {
  "dev": "tsx watch src/index.ts",
  "build": "tsc",
  "start": "node dist/index.js"
}
```

**What each does:**

- `npm run dev`: Runs your server with hot reload (like nodemon in MERN)
- `npm run build`: Compiles TypeScript to JavaScript
- `npm start`: Runs the compiled JavaScript (for production)

### Step 5: Create Your First File

```bash
mkdir src
```

Create `src/index.ts`:

```typescript
import express from 'express';

const app = express();
const PORT = 5000;

app.get('/ping', (req, res) => {
  res.json({ message: 'Server is running' });
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

### Step 6: Test It

```bash
npm run dev
```

Visit `http://localhost:5000/ping` in your browser. You should see:

```json
{"message":"Server is running"}
```

**Congratulations—you now have a TypeScript Express server running!** This is your foundation.

---

## Workflow

```bash
# Start dev server (terminal 1)
npm run dev

# Make changes to .ts files → nodemon auto-restarts

# Modify database schema → 
npx prisma migrate dev --name description

# Check your database visually (optional)
npx prisma studio
```

