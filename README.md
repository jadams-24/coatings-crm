# Coatings CRM

**A mobile-first CRM for a commercial flooring and coatings contractor — designed so reps actually use it, and so deals can't advance until the things that lose bids are handled.**

Built by the owner/seller of a coatings contracting business after off-the-shelf CRMs failed the same way they always do: reps don't log activity, and bids get lost to missed paperwork rather than price.

---

## The problem

A specialty contractor sells through four very different motions:

| Pipeline | What it is |
|---|---|
| **Public bid** | Published municipal and state bids with hard due dates, addenda, and mandatory pre-bid walks |
| **GC chase** | Getting carried on general contractors' bid lists |
| **Facility** | Selling direct to plant and facility owners |
| **Federal** | SAM.gov opportunities (set-aside eligible) |

Generic CRMs blend these into one pipeline, which makes win rates meaningless. And they rely on reps filling in fields, which they don't do on a job site.

## What it does

- **Gates over fields.** A deal can't move to the next stage until the real failure points are cleared: plans link uploaded, addenda acknowledged, bid amount entered, bond arranged. The UI shows every unmet condition as a checklist.
- **Automatic disqualification.** If a mandatory pre-bid walk passes without being completed, the opportunity is flagged as disqualified, because legally you can't bid it.
- **Two-tap logging.** A floating quick-log button records a call, visit, or email in two taps on a phone. Logs are saved offline and synced when the connection returns.
- **Cadence engine.** Calculates each opportunity's and contact's next action and urgency from pipeline-specific follow-up rules, so the daily view tells a rep exactly who to call today.
- **Four pipelines, never blended.** Stages and win rates are computed per motion.
- **Federal workflow.** Pulls SAM.gov opportunities, uses Claude to extract and score each solicitation, and caches USASpending award data for pricing intel.
- **Owner dashboards.** KPIs, goals, a bid calendar, and reports for the owners, separate from the rep's mobile view.

## How it's built

**Stack:** React 19 + TypeScript, Vite, Tailwind, installable PWA · Supabase (Postgres, auth, row-level security) · Vercel

Design decisions worth noting:

- **Pure-function rules engines.** Gate logic and cadence logic are side-effect-free TypeScript modules with unit tests, separate from the UI and database.
- **Derived on read, never persisted.** Disqualification and next actions are calculated when data is read, so they can never go stale or drift out of sync.
- **Database-enforced integrity.** Row-level security isolates each rep's records; triggers reject pipeline-specific data attached to the wrong pipeline; SQL invariant tests in `supabase/tests/` verify both.
- **Idempotent offline sync.** Queued activities carry client-generated IDs, so a retried sync never creates duplicates.

The full build spec is in [`coatings-crm-build-spec.md`](coatings-crm-build-spec.md).

## Results

<!-- Jordan: replace with your real numbers -->
- **[Activity logging went from X to Y per week]**
- **[N bids tracked / $ pipeline managed]**
- **[Missed-requirement losses: before vs. after]**

## Run it locally

```bash
npm install
cp .env.example .env.local   # add your own Supabase URL and publishable key
npx supabase db push         # apply migrations to your own Supabase project
npm run dev
npm test
```
