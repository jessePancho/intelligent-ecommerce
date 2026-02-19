# 🛠 Project Dev Log: Prisma 7 & Next.js Marketplace Setup

**Date:** February 19, 2026
**Status:** Phase 1 Foundation Complete ✅

## 🚀 Overview

Today we successfully configured the core database layer using **Prisma 7.4.0**. This version introduces a "Rust-free" architecture and a strict separation between data models and connection logic.

---

## 🏗 Key Architectural Decisions

### 1. Prisma 7 Configuration Shift

Prisma 7 no longer allows `url = env("DATABASE_URL")` inside `schema.prisma`.

- **What we did:** Moved the connection logic to a new root file: `prisma.config.ts`.
- **Reason:** P1012 Validation Error. Prisma 7 requires the schema to be a "pure" definition of data, while the config handles environment-specific URLs.

### 2. Local Database Engine

We are using the new **Prisma Dev Server** (`npx prisma dev`) instead of a traditional local Postgres installation.

- **Protocol:** `prisma+postgres://`
- **Behavior:** This database runs in memory/local storage via the terminal. It is **volatile** (data doesn't move across devices automatically).

### 3. Connection Tunneling

Because the `prisma+postgres` protocol is new, **Prisma Studio** does not support it directly yet.

- **Solution:** We used `@prisma/ppg-tunnel` to create a standard PostgreSQL bridge (TCP) on a local port (e.g., `45885`) to allow Studio to view the data.

---

## 📂 File Reference

### `prisma/schema.prisma`

```prisma
generator client {
  provider = "prisma-client" // Modern v7 provider
  output   = "../src/generated/prisma"
}

datasource db {
  provider = "postgresql"
  // NOTE: No 'url' here! Handled by prisma.config.ts
}

model Product {
  id          String   @id @default(cuid())
  name        String
  description String
  price       Float
  category    String
  stock       Int      @default(0)
  images      String[]
  createdAt   DateTime @default(now())
}
```

### `prisma.config.ts`

```prisma
import "dotenv/config";
import { defineConfig, env } from "prisma/config";

export default defineConfig({
schema: "prisma/schema.prisma",
datasource: {
url: env("DATABASE_URL"),
},
});
```

## 🚀 Daily Workflow (The "3-Terminal" Setup)

### 1. Terminal 1 (DB Engine)

`npx prisma dev`
Keep this running. If you restart it, update the URL in .env

### 2. Terminal 2 (Web Server)

`npm run dev`
Runs the Next.js frontend at localhose:3000

### 3. Terminal 3 (Studio/Tunnel)

`npx @prima/ppg-tunnel`
Then run Studio using the generated port:
`npx prisma studio --url "postgresql://127.0.0.1:[PORT]/postgres?sslmode=disable"`

## 🏠 Moving to a New Device (Home Setup)

### 1. Gitpull

the latest code.

### 2. NVM:

Switch to Node 24 `nvm use 24`

### 3. Install:

`npm install`

### 4. Start DB:

Run `npx prisma dev` and copy the New URL into your local `.env`

### 5. Sync Schema:

Run `npx prisma db push`.

### 6. Seed Data

Run `npx tsx prisma/seed.ts` to re-populate products.
