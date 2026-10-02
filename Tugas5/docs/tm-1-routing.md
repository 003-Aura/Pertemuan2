# TM-1 — Peta Routing Pribadi: Entity BUKU

| US-XX | METHOD + /api/v1/... | SvelteKit File | ATLAS SPA | Flutter Name + Args |
|---|---|---|---|---|
| US-01 | `GET /api/v1/buku` | `routes/buku/+page.svelte` | `/buku` | `getBooks()` |
| US-02 | `GET /api/v1/buku?search=...` | `routes/buku/+page.svelte` | `/buku?search=...` | `searchBooks(query)` |
| US-03 | `GET /api/v1/buku/:id` | `routes/buku/[id]/+page.svelte` | `/buku/:id` | `getBookById(id)` |
| US-04 | `POST /api/v1/buku` | `routes/buku/baru/+page.svelte` | `/buku/baru` | `createBook(data)` |
| US-05 | `PUT /api/v1/buku/:id` | `routes/buku/[id]/edit/+page.svelte` | `/buku/:id/edit` | `updateBook(id, data)` |