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

import "dotenv/config";
import { defineConfig, env } from "prisma/config";

export default defineConfig({
schema: "prisma/schema.prisma",
datasource: {
url: env("DATABASE_URL"),
},
});
