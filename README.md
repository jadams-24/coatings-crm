# Coatings CRM

**A sales dashboard and pipeline tracker for a commercial flooring and coatings contractor — built so deals can't advance until the things that lose bids are handled.**

Built by the owner/seller of a coatings contracting business after off-the-shelf CRMs failed the same way they always do: one blended pipeline that hides what's really happening, and bids lost to missed paperwork rather than price.

---

## The problem

A specialty contractor sells through four very different motions:

| Pipeline | What it is |
|---|---|
| **Public bid** | Published municipal and state bids with hard due dates, addenda, and mandatory pre-bid walks |
| **GC chase** | Getting carried on general contractors' bid lists |
| **Facility** | Selling direct to plant and facility owners |
| **Federal** | SAM.gov opportunities (set-aside eligible) |

Generic CRMs blend these into one pipeline, which makes win rates meaningless, and they don't stop a bid from going out with a missing requirement.

## What it does

- **Sales dashboard.** KPIs, goals, win rates, and reports computed separately for each pipeline, never blended.
- **Gates over fields.** A deal can't move to the next stage until the real failure points are cleared: plans link uploaded, addenda acknowledged, bid amount entered, bond arranged. Every unmet condition appears as a checklist.
- **Automatic disqualification.** If a mandatory pre-bid walk passes without being completed, the opportunity is flagged as disqualified, because legally you can't bid it.
- **Cadence engine.** Calculates each opportunity's and contact's next action and urgency from pipeline-specific follow-up rules, feeding a daily view of who to call today.
- **Bid calendar.** Every due date and pre-bid walk in one place.
- **Federal workflow.** Pulls SAM.gov opportunities, uses Claude to extract and score each solicitation, and caches USASpending award data for pricing intel.

## How it's built

**Stack:** React 19 + TypeScript, Vite, Tailwind · Supabase (Postgres, auth, row-level security, Edge Functions) · Anthropic API · Vercel

Design decisions worth noting:

- **Pure-function rules engines.** Gate logic and cadence logic are side-effect-free TypeScript modules with unit tests, separate from the UI and database.
- **Derived on read, never persisted.** Disqualification and next actions are calculated when data is read, so they can never go stale or drift out of sync.
- **Database-enforced integrity.** Row-level security isolates each user's records; triggers reject pipeline-specific data attached to the wrong pipeline; SQL invariant tests in `supabase/tests/` verify both.

The full build spec is in [`coatings-crm-build-spec.md`](coatings-crm-build-spec.md).

## Results

<!-- Jordan: replace with your real numbers -->
- **[N bids tracked / $ pipeline managed]**
- **[Time saved on pipeline reporting: X hours/week]**
- **[Missed-requirement losses: before vs. after]**

## Run it locally

```bash
npm install
cp .env.example .env.local   # add your own Supabase URL and publishable key
npx supabase db push         # apply migrations to your own Supabase project
npm run dev
npm test
```
