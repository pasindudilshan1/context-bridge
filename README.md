# context-bridge

**A Claude skill that generates a precise, pasteable Context Block — carrying your full project state into any AI tool instantly.**

No more re-explaining your stack. No more context drift. No more AI contradicting decisions you already made.

---

## The Problem

You're mid-project. You switch from Claude to Cursor. Or you start a new chat. And you spend the next 10 minutes re-explaining:

- Your stack
- Why you chose it
- What you already built
- What you're NOT using and why
- What the next task is
- What rules the AI has to follow

Every re-explanation introduces drift. The new AI makes slightly different assumptions. It suggests alternatives you already ruled out. It uses patterns inconsistent with what's already built.

**That's the problem context-bridge solves.**

---

## What It Does

context-bridge generates a **Context Block** — a single, copyable block you paste at the top of any AI conversation to give it instant, complete project context.

It covers:
- Stack and architecture
- Decisions already made (closed — do not revisit)
- Hard constraints and rules
- What's built and what's not
- Exactly what task you're doing next
- A compressed Memory Block for zero drift

It formats the block specifically for the target tool — Cursor, ChatGPT, Gemini, v0, Bolt, Claude Code, GitHub Copilot, Windsurf, Replit, Perplexity. Or uses a universal format for anything else.

---

## Modes

### Generate Mode (default)
You're mid-conversation. Ask Claude to generate a Context Block for your next tool. Claude extracts everything from the session, asks at most 3 targeted questions, and outputs the block.

### Decompile Mode
Paste a messy AI conversation. Claude extracts the clean context from it, strips the noise, resolves conflicts, and gives you a structured block.

---

## Install

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/[your-username]/context-bridge.git ~/.claude/skills/context-bridge
```

Restart Claude. The skill activates automatically when you switch tools or mention context loss.

---

## Usage Examples

**Generate a handoff to Cursor:**
> "Generate a context block for Cursor. I'm about to build the auth middleware."

**Decompile a messy chat:**
> "Extract a context doc from this conversation: [paste chat]"

**Take your project to a new Claude session:**
> "Make a context block so I can continue this in a new chat."

**Hand off to ChatGPT:**
> "Context block for ChatGPT — I need to continue the API route work there."

---

## Output Example

````
```context-block
## 🔁 Context Block — Invoice App → Cursor
[Generated at: 2025-06-12]

### Project
A SaaS invoicing tool for freelancers. Generates and sends PDF invoices, tracks payment status, connects to Stripe.

### Stack
- Frontend: Next.js 14, TypeScript, Tailwind CSS
- Backend: Next.js API Routes
- Database: Supabase (PostgreSQL)
- Auth: Supabase Auth (magic link only)
- Payments: Stripe Checkout + Webhooks
- PDF: react-pdf

### Architecture & Patterns
Server Components by default. Client Components only when interactivity requires it. API routes for all Stripe and Supabase mutations.

### Decisions Made (Do Not Revisit)
1. Auth: Magic link only — no password auth
2. PDF generation: react-pdf, not Puppeteer
3. No Redux — Zustand for global state, React Query for server state
4. Stripe Checkout hosted page — no custom payment form

### Constraints (Hard Rules)
1. No default exports anywhere
2. All Stripe webhook handlers must verify signature before processing
3. No client-side Supabase calls for mutations — always go through API routes
4. TypeScript strict mode — no `any`

### Coding Conventions
- Components: PascalCase, named exports
- Files: kebab-case
- Hooks: use[Name].ts pattern
- API routes: /app/api/[resource]/route.ts

### Current State
- Auth flow: complete
- Invoice creation form: complete
- PDF generation: complete
- Stripe Checkout redirect: complete
- Webhook handler: scaffold only — not processing events yet

### What's Not Done Yet
- Webhook handler logic
- Payment status updates in DB
- Invoice list page
- Email sending

### Your Task Right Now
Build the Stripe webhook handler at `/app/api/webhooks/stripe/route.ts`. It must: verify the Stripe signature, handle `checkout.session.completed` by updating the invoice status to "paid" in Supabase, and return 200. No other event types needed yet.

### Files to Know
- `/app/api/invoices/route.ts` — invoice creation endpoint
- `/lib/stripe.ts` — Stripe client instance
- `/lib/supabase.ts` — Supabase client
- `/types/invoice.ts` — Invoice type definitions

### Do Not Touch
- `/app/api/auth/` — auth routes, complete
- `/components/pdf/` — PDF components, complete

### Memory Block
stack: Next.js 14 + TypeScript + Tailwind + Supabase + Stripe
auth: magic link only, Supabase Auth
no-default-exports: true
strict-typescript: true
mutations: API routes only, no client-side Supabase
state: Zustand (global) + React Query (server)
pdf: react-pdf
```
````

> **How to use:** Copy everything inside the code block above and paste it at the very start of your first message in Cursor.

---

## Why Not Just Paste the Old Chat?

- Old chats include failed attempts, tangents, and noise
- They're too long to paste into tools with context limits
- The target AI tries to "help" by re-interpreting everything
- Decisions aren't clearly marked as closed — the AI re-opens them
- There's no clear task statement — the AI guesses

context-bridge extracts the signal, drops the noise, marks decisions as closed, and gives the target AI exactly what it needs to start immediately.

---

## Tool Support

| Tool | Profile Available |
|---|---|
| Cursor | ✅ |
| Windsurf | ✅ |
| ChatGPT / GPT-4o | ✅ |
| Gemini / Gemini Advanced | ✅ |
| v0 (Vercel) | ✅ |
| Bolt / Bolt.new | ✅ |
| Claude Code | ✅ |
| GitHub Copilot | ✅ |
| Replit AI | ✅ |
| Perplexity | ✅ |
| Any other tool | ✅ Universal format |

---

## License

MIT License. See [LICENSE](./LICENSE).

---

## Changelog

**1.0.0**
- Generate Mode with 10 tool profiles
- Decompile Mode with conflict resolution
- Universal Format for unknown tools
- Memory Block generation

---

*"The best handoff is the one where the next AI never has to ask what happened before."*
