# Vantage — Where I Am & What's Next

Last updated: 5 August 2026

---

## ✅ Done

- Vantage built as a desktop web app, deployed and live at bucketswealth.com
- Three calculators live at `/tools.html` with matching sidebar
- Sample mode ("See a sample week") working
- Waitlist popup working
- **GA4 fully configured and verified** — 14-month retention, Enhanced
  measurement on, `waitlist_submitted` and `app_entered` starred as key
  events, live data confirmed flowing

## ❌ Not done — this is what's left

- Clarity not connected (recording nothing)
- Formspree not connected (**emails are not being saved**)
- Search Console not set up
- 3 updated files not yet deployed (exit survey, feedback removal)
- Ads not started

---

# PHASE A ✅ COMPLETE

## Step 1 — Microsoft Clarity ✅
Project ID `xyd6ag4bls`, wired into `index.html` and `tools.html`.

## Step 2 — Formspree ✅
Form "Vantage waitlist", endpoint `https://formspree.io/f/mwlepqao`,
wired into both pages. Free tier = 50 submissions/month.

## Step 3 — Google Search Console ✅ DONE

Already set up and verified. Google auto-verified ownership using the
existing GA4 tag on the site, so no meta tag was needed.

Optional while you're there: paste `https://www.bucketswealth.com/` into
the **Inspect any URL** bar → **Request indexing**. Repeat for
`https://www.bucketswealth.com/tools.html`.

---

# PHASE B ✅ COMPLETE — files updated

All placeholders replaced. Nothing left to configure in the code.

**→ Next action is PHASE C: upload to GitHub.**

---

# PHASE C — Deploy (~10 min)

1. Duplicate the folder → rename the copy `backup-<today's date>`
2. GitHub repo → **Add file → Upload files**
3. Drag in **four files only**:
   - `index.html`
   - `tools.html`
   - `feedback.html`
   - `NEXT-STEPS.md`

   Do NOT upload `vantage-prototype-main/`, the `.zip`, or `.DS_Store`
4. Commit message: `Connect analytics, add exit survey, remove feedback form`
5. **Actions** tab → wait for the green tick (~2 min)

Nothing changes on GoDaddy — ever. `CNAME` already handles the domain.

**Rollback if needed:** GitHub → Commits → previous commit → **Revert**

---

# PHASE D — Verify live (~15 min)

On `www.bucketswealth.com` in a clean browser window (no ad blockers):

- [ ] Landing page loads
- [ ] **See a sample week** → populated app with a real move
- [ ] Waitlist popup appears after ~4s
- [ ] Submit a real email → **check it arrives in Formspree** ← proves the chain
- [ ] `localStorage.clear()` in console, reload, reach a move again
- [ ] Click **No thanks** → "What stopped you?" survey appears
- [ ] Pick a reason → modal closes
- [ ] Sidebar → **Tools** → three calculators, charts visible
- [ ] `bucketswealth.com/feedback.html` → redirects to Vantage (no Google Form)
- [ ] Clarity → **Recordings** → your session appears (~10 min lag)
- [ ] Search Console → click **Verify** → succeeds
- [ ] Search Console → submit `https://www.bucketswealth.com/` for indexing

Once this passes, the plumbing is complete and you can spend money.

---

# PHASE E — Launch ads ($180)

## Before you start

Note today's date. Everything in GA4 before it is your own testing and
should be excluded when reading results.

## Reddit — $150 over 11 days ($14/day)

1. **ads.reddit.com** → sign up → **Create campaign**
2. Objective: **Traffic**
3. Bid strategy: **CPC** (manual), max bid **$1.50**
4. Location: **United States only**
5. Targeting: choose **Communities**, not interests. Add:
   `r/personalfinance`, `r/HENRYfinance`, `r/financialindependence`,
   `r/MiddleClassFinance`
6. Create **two ads**:

   **Ad A — the restraint angle**
   > You earn $200K and still don't know if you're doing this right.
   > Vantage gives you one ranked money move a week. That's it.

   **Ad B — the anti-app angle**
   > Not another budgeting app. Vantage looks at your whole picture and
   > tells you the single highest-impact move this week — and shows the math.

7. Landing URLs — **split the budget between these two**:

```
Ad A → https://www.bucketswealth.com/?utm_source=reddit&utm_medium=cpc&utm_campaign=vantage_test&utm_content=a

Ad B → https://www.bucketswealth.com/?demo=1&utm_source=reddit&utm_medium=cpc&utm_campaign=vantage_test&utm_content=b_demo
```

The `?demo=1` link skips the landing page and drops people straight into
sample mode. Comparing these two is a real experiment: it tells you whether
the barrier is the pitch or the onboarding.

## Google Search — $30 over 11 days

1. **ads.google.com** → new **Search** campaign
2. Bidding: **Manual CPC**, max **$1.50**. Do NOT use "Maximise clicks"
3. Match types: **exact or phrase only** — broad match will burn $30 in two days
4. Keywords: `"personal cfo"`, `"financial advisor alternative"`,
   `"rent vs buy calculator"`
5. Landing URL:

```
https://www.bucketswealth.com/tools.html?tool=rvb&utm_source=google&utm_medium=cpc&utm_campaign=vantage_test
```

Low volume expected — this positioning has little search demand. Defensive only.

## While it runs

Check daily for the first 3 days (catching a broken pixel on day 1 vs day 8
is the difference between a valid test and a wasted $180). Then leave it
alone. You don't have the volume to optimise your way out of noise.

---

# PHASE F — Read the results

## Daily
Watch **5 Clarity recordings** of people who did NOT reach a weekly move.
This will teach you more than every dashboard combined.

## Weekly

| Metric | Dead | Promising | Strong |
|---|---|---|---|
| Started onboarding or sample | <25% | 35–55% | >65% |
| Reached a weekly move | <15% | 25–40% | >50% |
| Completed all 4 stages *(real mode)* | <10% | 20–35% | >45% |
| Waitlist emails | 0–2 | 8–15 | 20+ |

**Headline metric:** `waitlist_submitted ÷ vantage_landing`

**Always filter by `mode`** (`sample` vs `real`). Combined they're meaningless —
a high `app_entered` count could just be people clicking the demo.

**Main diagnostic:** the drop across `onboarding_stage_viewed` 1→2→3→4.
Expect a cliff at the income/401(k) stage. If it's there, push traffic to the
`?demo=1` link — don't rewrite the ads.

**Second diagnostic:** `exit_survey_answered` reasons. At this sample size,
the *reason* people decline is worth more than the count. "Privacy" means fix
trust. "Not relevant" means fix targeting. "Too early" means the product is
fine and the moment is wrong.

## Budget escalation

$180 is a smoke test — it detects signal, it doesn't measure it. If results
come back ambiguous rather than dead, top up to ~$500 total for a real
go/no-go read. Decide that in advance so you're not choosing under
sunk-cost pressure.

---

# The stack

| Tool | Cost | Answers |
|---|---|---|
| GA4 | $0 | What happened, how many, from where |
| Clarity | $0 | *Why* people quit — recordings + heatmaps |
| Formspree | $0 | Who signed up + why others didn't |
| Search Console | $0 | What people search before landing |
| Reddit Ads | $150 | Primary traffic |
| Google Ads | $30 | Defensive intent keywords |

**Total: $180**

---

# Known rough edges

- Tax constants in `tools.html` are still 2024 US values. Vantage's are 2026.
  Worth aligning before driving traffic to both.
- No data persists on refresh — by design for a prototype, but "Back to
  Vantage" from Tools returns to the landing page, not where you left off.
- Compound calculator markup is dead code inside `tools.html`.
- Formspree free tier caps at 50 submissions/month.
