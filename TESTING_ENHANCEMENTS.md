# Enhancement Test Checklist (TEMPORARY — delete before opening the upstream PR)

Everything below covers **only the features added on this branch** (`atd_enh_*`),
not the base mod. Each section says what to do, what you should see, and what to
tell Claude if it misbehaves — several behaviors rest on assumptions that could
not be verified outside the game, and the fix differs depending on *how* it fails.

**Setup reminders:** enable Workshop CMF first, then this fork ("Autonomous
Diplomats Dev"); disable the Workshop Autonomous Diplomats (same `atd_`
namespace). After any test session, check
`...\Paradox Interactive\Europa Universalis V\logs\error.log` for `atd_enh` spam.

**Best test nation:** one that is the **dominant country of its own culture**
(required for both cultural-view features), with a decent treasury, some
diplomats, at least one **subject that leads a different culture**, and a mix of
small + great-power culture-leader neighbors.

---

## 1. UI presence (5 minutes, do first)

- [ ] **Improve Relations tab → Action Priority list has 14 rows.** Rows 12–14 are
      **Curry Favors / Send Gifts / Culture Improve**, each with a count slider
      (0–10) and drag-to-reorder like the others.
- [ ] **Subjects tab → Subject Actions list has 3 rows.** Row 3 is **Improve
      Cultural View** with its own Enabled toggle.
- [ ] **Subjects tab → Options group** exists with **"Enforce Culture: Skip
      Culture Leaders"**.
- [ ] **Favors tab** exists with the **Gifts** group: Culture Leaders Only
      (default on), Max Treasury Per Month (default 25%), Notify When Giftable
      (default off).
- [ ] **Tooltips** show on hover for rows 12–14, Subject Actions rows 1–3, and
      each Favors/Options setting.
- [ ] All new toggles/sliders **persist through save/reload**.

If a row/tab is missing entirely → registration ordering problem; tell Claude
which piece is missing.
If rows 12–14 exist but show raw keys like `atd__improve_relations_i12_name` →
localization file isn't loading.

## 2. Curry Favors (priority row 12)

Set its count to 2–3, everything else 0.

- [ ] With count **0** nothing fires; with count > 0, up to that many
      **Influence Nation** actions fire per month against **non-great-power
      culture-leader** nations (their favors owed to you rise by +5 each).
- [ ] **Great-power** culture leaders are never curried.
- [ ] **Stops at 25:** once a nation owes you **≥ 25 favors**, curry stops
      targeting it (it should plateau around 25).
- [ ] Nations flagged with the base mod's **Block Autonomous Improving** are
      skipped.

⚠️ **Favor-direction check (critical):** if curry *never* fires at all, or keeps
firing far past 25, the favor-reading trigger direction is inverted — tell
Claude ("curry ignored the 25 cap" vs "curry never fired") and it's a one-line
swap (`prev_favors_with_this` ↔ `this_favors_with_prev`).

## 3. Send Gifts (priority row 13) — least-certain feature

Set its count to 2–3; note your treasury first.

- [ ] **3a. Does it fire at all?** Gifts are sent (treasury drops; targets'
      favors/opinion toward you rise). **If nothing happens, this action cannot
      be script-triggered** — tell Claude; the fallback is replicating the gift
      manually (spend gold + grant favors via script), which also unlocks true
      %-of-gift-size control.
- [ ] **3b. Spend cap.** With Max Treasury at 25%, gifting stops once treasury
      falls ~25% below its month-start value. Note roughly what % was spent.
- [ ] **3c. Targeting toggle.** Culture Leaders Only ON → only culture leaders
      receive gifts; OFF → any discovered nation in range may.
- [ ] **3d. Great powers reachable.** A great-power culture leader (which curry
      skips) receives gifts.
- [ ] **3e.** Treasury never goes negative; at most 20 gifts/month.

## 4. Culture Improve (priority row 14)

Needs a culture-leader target that owes you **≥ 50 favors** (build via curry +
gifts, or gift manually). Set count to 1–2.

- [ ] **4a. Favor cost enforced? (critical)** When it fires on nation X, your
      favors with X should **drop by 50** and X's culture's view of yours tick
      up one step. **If the view improves but favors do NOT drop, the action is
      performing for free** → tell Claude (we'd add manual favor accounting, and
      the whole build-favors pipeline gets re-checked).
- [ ] **4b.** With **< 50 favors** and no other source, it does **not** fire on
      that target.
- [ ] **4c.** Skips cultures already at **kindred** toward yours.
- [ ] **4d.** Does nothing while you are **not** the dominant country of your
      own culture.

## 5. Improve Subject Cultural View (Subject Actions row 3)

Needs a loyal subject (loyalty ≥ 50, liberty desire < 50) that is the dominant
country of its culture. Toggle the row on.

- [ ] **5a.** The subject's culture's view of yours improves one step, and the
      subject gains a little liberty desire.
- [ ] **5b. Cooldown honored (critical):** the same subject is **not** hit again
      next month — it should be on the vanilla 50-year cooldown (check the
      vanilla subject-interaction UI shows it as on cooldown too). Two failure
      modes to report: "fires every month" (cooldown not registering — wrong
      scope) or "never fires even once" (cooldown check reading the wrong scope).
- [ ] **5c.** Since the base game already notifies when this action is available,
      confirm the vanilla notification still appears normally (this branch adds
      no duplicate alert for it).
- [ ] **5d.** Does nothing while you don't lead your own culture.

## 6. Skip Culture Leaders option (Subjects → Options)

With **Enforce Culture** row enabled and a culture-leader subject eligible:

- [ ] Toggle **ON** → Enforce Culture converts other subjects but **leaves
      culture-leader subjects alone** (so row 3 can handle them).
- [ ] Toggle **OFF** → Enforce Culture converts culture-leader subjects as
      vanilla base-mod behavior would.

## 7. Shared diplomat pool + priority (rows 12–14)

- [ ] **7a. Diplomat consumption (critical):** watch your free-diplomat count
      when a curry/gift/culture action fires. **Does it drop by 1 per action?**
      The code assumes the engine consumes one; if the count does NOT drop, tell
      Claude → an explicit `add_diplomats = -1` gets added so the reserve and
      priority actually bind.
- [ ] **7b.** Set **Minimum Diplomats Reserve** above your free diplomats → all
      three favor actions stop, same as the relation categories.
- [ ] **7c.** Drag Culture Improve above a relation category, give both counts,
      run with scarce diplomats → the higher row acts first.

## 8. "Ready for Gift Top-Up" alert (Favors → Notify When Giftable)

- [ ] Fires (yellow alert) when some culture-leader nation owes you **25–49
      favors** and you have ≥ 25 gold; clears when nothing is in that window or
      the toggle is off. Right-click dismisses. Works with all automation
      counts at 0.
- [ ] If it fires always / never regardless of favors → same favor-direction
      swap as §2.

## 9. Base-mod regression (quick pass)

The base files were touched in only 6 marked spots, but confirm nothing broke:

- [ ] Relation categories (rows 1–11) still improve relations normally,
      including v1.4's **Largest Nations** and the **max relations / max
      antagonism** limits.
- [ ] **Enforce Religion / Enforce Culture** still work (with Skip Culture
      Leaders OFF, behavior is exactly vanilla base-mod).
- [ ] Block/Allow Autonomous Improving actions still work.
- [ ] No new `error.log` entries mentioning `atd` after a few game-years.

---

## Reporting back

For anything that fails, the useful bug report is: **which checkbox, what you
expected, what actually happened** (plus an error.log snippet if any). The
critical ones that decide code changes are **2 (favor direction), 3a (gift
scriptability), 4a (favor cost), 5b (cooldown scope), 7a (diplomat
consumption)** — the rest are tuning.

> Remember: `git rm TESTING_ENHANCEMENTS.md` (or ask Claude) before opening the
> upstream pull request.
