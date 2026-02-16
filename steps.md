# Project Roadmap: Portfolio Application

This roadmap follows an **"Inside-Out"** approach, establishing the data layer and state management before building the UI to ensure a scalable and bug-resistant architecture.

---

## Phase 1: The Data Backbone (`/src/types` & `/src/lib`)

_Goal: Define the "shape" of your application and establish core service connectivity._

- [ ] **Define TypeScript Interfaces:** Map out `product.ts`, `user.ts`, and `transaction.ts` in `/src/types`.
- [ ] **Initialize Prisma Schema:** Define models (User, Product, Order) and sync with your database (`npx prisma db push`).
- [ ] **Setup Service Clients:** Initialize singleton instances in `/lib` for:
  - `prisma.ts` (Database ORM)
  - `stripe.ts` (Payment Processing)
  - `pinecone.ts` (Vector Search Integration)

## Phase 2: State Management (`/src/redux`)

_Goal: Create the "Single Source of Truth" for the frontend._

- [ ] **Configure Redux Store:** Set up the main store provider and middleware in `store.ts`.
- [ ] **Auth Slice:** Implement `authSlice.ts` first (handles user sessions and permissions).
- [ ] **Cart & Inventory Slices:** Build the logic for managing shopping cart state and real-time stock levels.
- [ ] **Validation:** Verify state transitions using Redux DevTools.

## Phase 3: Core Layouts & Routing (`/src/app`)

_Goal: Establish the application "shell" to leverage Next.js partial rendering._

- [ ] **Store Layout:** Build `src/app/store/layout.tsx` (Customer-facing Navigation/Footer).
- [ ] **Vendor Layout:** Build `src/app/vendor/layout.tsx` (Admin/Seller Sidebar and Dashboard shell).
- [ ] **Loading & Error States:** Create `loading.tsx` and `error.tsx` files for better UX during data fetching.

## Phase 4: Component Architecture (`/src/components`)

_Goal: Build reusable UI "atoms" and complex functional "modules"._

- [ ] **UI Atoms:** Build foundational elements in `/ui` (Buttons, Inputs, Modals, Badges).
- [ ] **Functional Modules:** Compose complex components in `/modules` (Product Cards, Search Bars, Cart Drawers).
- [ ] **State Integration:** Connect these components to Redux via `useAppSelector` and `useAppDispatch`.

## Phase 5: API Logic & External Integration (`/src/api`)

_Goal: Connect the frontend to external services and server-side logic._

- [ ] **Search Integration:** Implement Pinecone logic in `search.ts` for vector-based discovery.
- [ ] **Stripe Webhooks:** Set up `payment.ts` to process secure transaction signals.
- [ ] **Global Route Logic:** Finalize shared API utilities and webhook handlers in `route.ts`.

---

### Development Strategy: The "Vertical Slice"

Instead of building all UI then all API, pick one feature (e.g., **"Add to Cart"**) and build it through every layer:

1. **Type:** Define the Product interface.
2. **Database:** Create the Product model in Prisma.
3. **Redux:** Create the `addToCart` action.
4. **UI:** Build the `ProductCard` and the "Add" button.
