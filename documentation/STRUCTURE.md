## Folder Structure

The project follows a `src`-centric approach, organizing application code distinctly from root-level configuration files.

### `/src`

- **/app**: The heart of the routing engine.
  - **/store**: Defines the customer-facing layout.
    - `layout.tsx`: Customer-facing navigation and footer.
  - **/vendor**: Defines the administrative shell for vendors.
    - `layout.tsx`: Administrative layout for sellers.

- **/components**: Shared UI components.
  - **/ui**: Basic UI components like buttons and forms.
  - **/modules**: Complex components like product cards, banners, etc.

- **/lib**: Third-party client initializations and utility functions.
  - `prisma.ts`: Database operations using Prisma ORM.
  - `pinecone.ts`: Operations for vector database integration (Pinecone).
  - `stripe.ts`: Payment processing with Stripe API.

- **/redux**: Houses the Redux logic and state management.
  - `cartSlice.ts`: Redux slice for cart state management.
  - `authSlice.ts`: Redux slice for user authentication.
  - `inventorySlice.ts`: Redux slice for inventory management.
  - `store.ts`: Redux store provider setup.

- **/types**: TypeScript interfaces and types for data consistency.
  - `product.ts`: Product-related TypeScript interfaces.
  - `user.ts`: User and session-related TypeScript interfaces.
  - `transaction.ts`: Transaction-related TypeScript interfaces.

- **/api**: Global API route definitions and logic.
  - `route.ts`: Defines global route logic for APIs (webhooks, search).
  - `search.ts`: API routes related to product search using Pinecone.
  - `payment.ts`: Payment-related API routes for Stripe webhooks.

---

### Rationale for Structure:

This structure facilitates "partial rendering" in Next.js, where navigation between dashboard sub-pages updates only the content area while preserving the sidebar state. The organization also ensures scalability and modularity by grouping related components, state logic, and utility functions. Private folders (e.g., `_lib`) house sensitive or internal logic that should not be exposed via the public API.
