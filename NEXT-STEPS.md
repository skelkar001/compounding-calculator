# Vantage — Build & Test Guide

Last updated: 30 July 2026

---

## ⚠️ FIRST: two renames (30 seconds, do this before testing)

The new files are sitting alongside the old ones so nothing was destroyed.
In Finder, inside this folder:

1. Rename `index.html` → `tools.html`
2. Rename `index-new.html` → `index.html`

That's it. After the rename:

| File | What it is |
|---|---|
| `index.html` | **Vantage** — the product (was `index-new.html`) |
| `tools.html` | The 3 calculators (was `index.html`) |
| `feedback.html` | Feedback form, re-themed |
| `CNAME` | Domain config — **never touch** |

---

## Test locally BEFORE uploading anything

Double-click `index.html`. It opens in your browser from your own machine —
the live site is untouched until you deliberately upload.

### A. Landing page

- [ ] Dark espresso hero, "Vantage / One decision a week. That's it."
- [ ] Two buttons: **Get started** and **See a sample week**
- [ ] A white preview card on the right showing an example move
- [ ] Prototype notice visible under the buttons

### B. Sample mode (this is the critical new path)

- [ ] Click **See a sample week**
- [ ] Lands straight in the app — no data entry at all
- [ ] Yellow banner at top: "You're viewing sample numbers"
- [ ] "This week's move" card shows a real, specific recommendation
- [ ] **See the math** expands the reasoning
- [ ] Left sidebar: Home / Net worth / Goals / Tools / Profile
- [ ] Net worth screen shows populated assets, liabilities, composition bar
- [ ] Goals screen shows 3 goals with progress rings
- [ ] After ~4 seconds the waitlist modal appears
- [ ] Click **Use my own numbers** → banner clears, onboarding starts

### C. Real onboarding

- [ ] Click **Get started** from the landing page
- [ ] Stage 1: form is **empty** (not pre-filled with sample data)
- [ ] The Plaid card is clearly labelled **DEMO ONLY**
- [ ] Click through the demo connection → balances fill in
- [ ] "Run basic optimizer" → quick wins screen
- [ ] "Next: set your goals" → goal rows
- [ ] "Project my plan" → projection with shortfall/surplus and 3 strategies
- [ ] "See my optimized moves" → enters the app

### D. Tools page

- [ ] Sidebar → **Tools** → opens `tools.html`
- [ ] **Same espresso sidebar** as the main app — should feel like a section
      of Vantage, not a different website
- [ ] Sidebar shows: Back to Vantage, then a TOOLS group with the three
      calculators as nav items
- [ ] Clicking each one switches the calculator and highlights correctly
- [ ] Page is cream/coral, **not** dark purple
- [ ] Complete a Rent vs Buy run → **charts are visible** (they were dark-themed before)
- [ ] "Back to Vantage" returns to the main app
- [ ] Waitlist modal here pitches Vantage, not a calculator

### E. Waitlist behaviour

- [ ] Appears once, then never again — **including across both pages**
- [ ] To reset for repeat testing: open console (Cmd+Opt+J) and run `localStorage.clear()`

### F. Responsive

- [ ] Narrow the window to phone width — sidebar becomes a top bar, columns stack

---

## What changed from the mobile prototype

**Ported unchanged:** the entire rules engine (`runEngine`), tax brackets,
projections, gap analysis, strategy ranking, goal maths. The recommendations
are identical to your prototype.

**Rebuilt for desktop:** phone frame removed, bottom tabs → left sidebar,
home screen → two columns, onboarding stages → wide centred cards,
sub-screens → modal overlays.

**Added:** sample mode, full funnel instrumentation, waitlist, trust
messaging, Tools link.

**Removed — and why:**

- **The Gemini AI chat.** It required each visitor to paste in their own
  Google API key, which is a non-starter for public traffic. Also the
  original source for it was truncated when I fetched the repo.
- **The compound interest calculator.** Vantage projects net worth already,
  and it was the weakest of the four. Its markup is still in `tools.html`
  but unreachable — safe to delete whenever.

**Reimplemented** (the fetch from GitHub truncated the last ~30 lines):
`toast()`, and the cadence helpers (`cadenceDescription`, `cadenceEyebrow`,
`cadenceShort`, `updateCadPreview`, `saveCadence`). Behaviour matches what
the prototype UI describes, but compare against your original if the wording
matters to you.

---

## Deploy when local testing passes

1. Back up: duplicate this whole folder as `backup-before-vantage`
2. GitHub → **Add file → Upload files**
3. Drag in: `index.html`, `tools.html`, `feedback.html`, `NEXT-STEPS.md`
4. Commit message: `Pivot to Vantage — decision engine as main product`
5. **Then delete the old file if GitHub still lists a stale one** — check the
   repo file list looks like the table at the top of this doc
6. Wait for the green tick in the **Actions** tab
7. Re-run the whole checklist above on the live site

Rollback: GitHub → Commits → the commit before yours → **Revert**.

---

## Still to configure (nothing works without these)

| Placeholder | Where | Get it from |
|---|---|---|
| `PASTE_CLARITY_PROJECT_ID_HERE` | `index.html`, `tools.html`, `feedback.html` | clarity.microsoft.com → Settings → Setup |
| `PASTE_FORMSPREE_ENDPOINT_HERE` | `index.html` **and** `tools.html` | formspree.io → new form |
| `share-image.png` | repo root | 1200×630 PNG, Canva |

Until pasted, the modal still opens and all events still fire — submissions
just log to the browser console. So you can test the full flow today.

### GA4 settings (no code)

- [ ] Admin → Data retention → 2 months → **14 months** (do this first)
- [ ] Admin → Data streams → **Enhanced measurement** on
- [ ] Admin → Key events → add `waitlist_submitted`, `app_entered`

---

## The funnel you're now measuring

```
vantage_landing
   ├── sample_mode_started ──→ app_entered ──→ waitlist_shown → waitlist_submitted
   └── onboarding_started
         └── onboarding_stage_viewed (1 → 2 → 3 → 4)
               └── app_entered ──→ waitlist_shown → waitlist_submitted
```

**Every event carries `mode: sample | real`.** Never read them combined —
a high `app_entered` count could just be people clicking the demo.

Other events: `quick_wins_viewed`, `goals_set`, `plan_viewed`,
`weekly_move_seen`, `see_the_math_opened`, `move_accepted`, `move_dismissed`,
`tool_opened`, `back_to_vantage`, `sample_exit_to_real`.

**Headline metric:** `waitlist_submitted ÷ vantage_landing`

**Main diagnostic:** the drop between `onboarding_stage_viewed` 1→2→3→4.
Expect a cliff at the stage asking for income and 401(k) balance. If it's
there, the answer is to push more traffic through sample mode — not to
rewrite the ads.

---

## Revised test thresholds

Ads now land directly on Vantage, which is a bigger ask than a calculator.
Judge against these:

| Metric | Dead | Promising | Strong |
|---|---|---|---|
| Started onboarding OR sample | <25% | 35–55% | >65% |
| Reached a weekly move (any mode) | <15% | 25–40% | >50% |
| Completed all 4 stages (real mode) | <10% | 20–35% | >45% |
| Waitlist rate (of those who saw a move) | 0–2 | 8–15 | 20+ |

**Budget:** Reddit $150 / Google $30. Google Search has very little intent
volume for this positioning — treat it as defensive only. Reddit creative
should lead with the restraint ("one decision a week"), not the feature list.

**Ad landing URLs:**

```
Straight to the product:  ...bucketswealth.com/?utm_source=reddit&utm_medium=cpc&utm_campaign=vantage_test
Skip to the demo:         ...bucketswealth.com/?demo=1&utm_source=reddit&utm_medium=cpc&utm_campaign=vantage_demo
Calculator entry point:   ...bucketswealth.com/tools.html?tool=rvb&utm_source=google&utm_medium=cpc
```

That second URL is worth an A/B test on its own — it removes every barrier
between the ad click and seeing value.

---

## Known rough edges

- The `tools.html` re-theme was a **token swap plus a targeted pass** on the
  charts and glows. Most of it looks right; if you spot a stray dark-theme
  artefact, tell me where and it's a one-line fix.
- Tax constants in `tools.html` are still 2024 US values. Vantage's are 2026.
  Worth aligning before you spend money driving traffic to both.
- No data persists on refresh — by design for a prototype, but it means a
  returning visitor starts over. Fine for the test; not fine for a product.
- The compound calculator markup is dead code inside `tools.html`.
