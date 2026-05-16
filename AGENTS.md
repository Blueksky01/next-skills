# Agent Instructions — CSPTS

## Stack
- Next.js 15 App Router, TypeScript strict (no `any`)
- MongoDB/Mongoose, GCS storage, NextAuth v5
- Roles: Student | Advisor | Admin

## Rules
- No `any` — use proper TypeScript types
- No emojis in code or markdown
- API routes: check session + role before DB ops
- DB: call `connectDB()` from `src/lib/db` before any Mongoose operation
- Storage: use `src/lib/storage.ts` functions only
- Auth: `getServerSession(authOptions)` — never trust client-sent role claims

## Code examples
```ts
// Bad: skips auth check
export async function POST(req: Request) {
  await connectDB();
  const body = await req.json();
  await Project.create(body);
}

// Good: checks session first
export async function POST(req: Request) {
  const session = await getServerSession(authOptions);
  if (!session) return new Response("Unauthorized", { status: 401 });
  await connectDB();
  const body = await req.json();
  await Project.create({ ...body, owner: session.user.id });
}
```

## Quick reference

| Rule | Yes/No |
|---|---|
| TypeScript `any` | No |
| Direct MongoDB driver | No |
| Mongoose models | Yes |
| Client direct GCS upload | No |
| Emojis in code/comments | No |
