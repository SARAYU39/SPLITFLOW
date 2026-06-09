# SplitFlow

**Split bills, not privacy.**

SplitFlow is a privacy-first expense-splitting app — a modern alternative to Splitwise built around one simple idea: you should be able to split bills with friends without handing over your phone number or email. Instead of contacts, SplitFlow gives every user a unique alphanumeric ID (like `SPL8X7K92`) so you can connect with people securely and anonymously.

---

## Features

- **Anonymous connections** — share a short ID like `SPL8X7K92` instead of a phone number or email. No contact list, no personal data required to add someone to a group.
- **Smart settlements (Greedy Debt Minimizer)** — instead of a dozen back-and-forth payments, SplitFlow calculates everyone's net balance and generates the *minimum* number of transactions needed to settle the whole group.
- **Three split modes** — divide an expense **equally**, by **percentage**, or by **exact custom amounts**, with live validation so the splits always add up.
- **Receipt uploads** — attach a receipt (JPG, PNG, or PDF) to any expense.
- **Private by design** — every table is protected by row-level security, so you only ever see the groups, expenses, and settlements you're actually part of.
- **A UI that feels good to use** — frosted-glass panels floating over animated ambient blobs, with smooth micro-interactions throughout.

---

## How the Greedy Debt Minimizer works

Settling a group shouldn't mean everyone paying everyone. SplitFlow reduces a tangle of debts to the fewest possible transfers:

1. **Net every balance.** For each member, sum what they paid and what they owe to get a single net figure — positive (a creditor) or negative (a debtor).
2. **Sort both sides.** Debtors and creditors are ordered by amount.
3. **Match greedily.** The largest debtor pays the largest creditor. Whichever side is fully settled drops out; the remainder carries forward. Repeat until every balance is zero.

The result is the minimum number of payments required to clear the group — so four friends who'd otherwise owe each other six different amounts might settle in just two transfers.

---

## Tech stack

| Layer | Tools |
|-------|-------|
| **Frontend** | React 19, TypeScript, Tailwind CSS v4, Framer Motion |
| **Backend** | Supabase — Auth, PostgreSQL, and Storage (receipt bucket) |
| **Security** | PostgreSQL Row-Level Security on every table |

---

## Getting started

### Prerequisites

- [Node.js](https://nodejs.org/) (18 or newer recommended)
- A free [Supabase](https://supabase.com/) project

### 1. Clone and install

```bash
git clone https://github.com/YOUR-USERNAME/splitflow.git
cd splitflow
npm install
```

### 2. Configure environment variables

Copy the example file and fill in your Supabase credentials:

```bash
cp .env.example .env
```

```env
VITE_SUPABASE_URL=your-project-url
VITE_SUPABASE_ANON_KEY=your-anon-key
```

> Both values are in your Supabase dashboard under **Project Settings → API**.
> If your project uses Next.js instead of Vite, prefix these with `NEXT_PUBLIC_` instead of `VITE_`.

### 3. Set up the database

In your Supabase project, create the tables for users, groups, group members, expenses, expense splits, and settlements — and enable **Row-Level Security** on each one, with policies that scope every row to the authenticated user's group membership. Create a **Storage bucket** for receipts that accepts JPG, PNG, and PDF.

### 4. Run it

```bash
npm run dev
```

The app will be available at the URL printed in your terminal (typically `http://localhost:5173`).

---

## Security notes

- **Row-Level Security is the backbone.** Access control lives in the database, not just the UI — so even direct API calls can only return rows the user is entitled to. Keep RLS enabled on every table.
- **Never commit secrets.** The anon key is safe to ship to the client; your service-role key is not. Keep it out of the repo (`.env` is already in `.gitignore`).

---

## License

Released under the MIT License — see [LICENSE](LICENSE).
