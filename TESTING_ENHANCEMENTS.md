# Enhancement Test Checklist (TEMPORARY — delete before opening the upstream PR)

Covers **only the features added on this branch** (`atd_enh_*`). Redesigned after
the first playtest: the favor actions now live entirely on the **Favors tab**
(the Improve Relations list is back to the base mod's 11 rows).

**Watching what the mod does:** every enhancement action is now recorded to the
**CMF Mod Action Log** (open it from the Community Mod Framework menu). Entries
read like "Curried favors with France" / "Sent a gift to Fez" / "Improved
cultural view with Tunis". That's the fastest way to confirm what fired each
month. Cross-check favors on a target's diplomacy panel and your treasury.

**⚠️ Diplomat-wipe note from the first playtest:** the base mod's
**Settings → Enabled** toggle also activates the *game's own* "Diplomacy
Interactions" automation (that's base-mod behavior — its tooltip says so). With
a large bank of diplomats, the vanilla automation will happily spend them all
improving relations, which looks like an instant wipe. While testing the
enhancements, keep **Settings → Enabled OFF** (the Favors engine no longer
depends on it — it has its own master toggle) or set a high diplomat reserve.

**Setup reminders:** Workshop CMF first, then this fork; don't enable the
Workshop Autonomous Diplomats at the same time. Check
`logs\error.log` for `atd_enh` entries after each session.

**⚠️ Curry Favors availability:** the game only unlocks the Influence Nation
action for the **Diplomatic Hegemon** (`allow_diplomacy_influence_nation`
modifier). The mod checks this, so until you hold that hegemony **Curry Favors
will silently never fire — that is correct behavior, not a bug** — and gifts
are your only automated favor source. Early-game Granada will not curry.

**Round-2 changes to verify (fixes from your first UI pass):**
- Favor Targets list rows should now **render** (missing `_on_changed`
  scripted GUI added) and the section header should read **"Favor Targets"**
  instead of a raw key.
- "Culture Improve" renamed to **"Culture Improve Non-Subjects"**.
- **Curry Favors Up To** slider now maxes at **25**; **Send Gifts Up To** stays
  adjustable 0–100, default 50.
- **NEW: Automation Priority** list on the **Settings tab** — drag to order
  Subjects / Favors / Improve Relations (default in that order). The monthly
  pass runs them in that order, so the higher automation claims diplomats
  first. NOTE: with this change the Favors engine runs inside the base pass
  when **Settings → Enabled is ON**; when Enabled is OFF, Favors still runs
  standalone each month. (The earlier advice to keep Enabled OFF while testing
  favors still works — but to test *priority*, Enabled must be ON, which also
  activates the game's own diplomacy automation; set a diplomat reserve to keep
  it from draining your bank.)
- [ ] **Priority test:** with Enabled ON, give Subjects/Favors/Improve each
  some work and a scarce diplomat budget; check the Mod Action Log + outliner —
  the top-priority automation's actions should happen first. Reorder and
  verify the order flips.

---

## 1. UI presence (5 minutes, do first)

- [ ] **Improve Relations tab is back to the base 11 rows** (no Curry/Gifts/Culture rows).
- [ ] **Favors tab** has: **Automation** (Improve Favors master, Curry Favors, Send Gifts, Culture Improve, Culture Improves Per Month), **Limits** (Curry Favors Up To = 25, Send Gifts Up To = 50), **Favor Targets** list (9 draggable rows: Culture Leaders, Neighboring, Own Subjects, Outraged, Allies, Threatening, Market Owners, Kingdoms and Empires, Largest Nations — each with an Actions slider), and **Gifts** (Culture Leaders Only, Max Treasury Per Month, Notify When Giftable).
- [ ] **Subjects tab**: 3 Subject Actions rows; Options shows **"Enforce Culture: Skip Subject Culture Leaders"** (renamed).
- [ ] Tooltips on everything; settings persist through save/reload.

## 2. Favor engine basics

Enable **Improve Favors** + **Curry Favors**; set **Culture Leaders** category to 2–3 actions; everything else 0. Keep Settings → Enabled OFF.

- [ ] **2a.** Each month, Mod Action Log shows "Curried favors with X" entries (≤ the category count); those nations' favors owed to you rise +5.
- [ ] **2b.** Only culture-leader nations targeted (that's the only active category); **great powers skipped** (curry can't reach them).
- [ ] **2c. Stops at the curry cap** (25): a nation at ≥25 favors stops being curried. ⚠️ If curry never fires or blows past the cap → favor-direction triggers are inverted; tell Claude ("never fired" vs "ignored cap").
- [ ] **2d.** Category priority: give two categories counts, drag one above the other, verify with scarce diplomat budget the higher one logs first.
- [ ] **2e.** Master OFF → nothing fires regardless of other toggles.

## 3. Send Gifts method (least-certain feature)

Also enable **Send Gifts**; note your treasury.

- [ ] **3a. Does it fire at all?** Log shows "Sent a gift to X"; treasury drops; target favors rise. **If no gift entries ever appear**, `send_gift` isn't script-triggerable → tell Claude (fallback: manual gold→favors replication with true % sizing).
- [ ] **3b.** Gifts go to targets curry can't handle: great-power culture leaders, or nations between the curry cap (25) and gift cap (50).
- [ ] **3c.** **Max Treasury Per Month** at 25% → gifting stops once treasury falls ~25% below month-start. At 0% → no gifts.
- [ ] **3d.** **Gifts: Culture Leaders Only** ON → only culture leaders gifted even with other categories active; OFF → any eligible target.

## 4. Culture Improve

Get a culture leader to ≥50 favors owed. Enable **Culture Improve** (count ≥1); you must lead your own culture.

- [ ] **4a. Favor cost enforced? (critical)** Log shows "Improved cultural view with X"; favors with X **drop by 50**; X's culture's view of yours ticks up. **If the view improves without favors dropping → the action performs free**; tell Claude.
- [ ] **4b.** Doesn't fire below 50 favors; skips **kindred** cultures; does nothing when you don't lead your own culture.

## 5. Subject cultural view (Subjects tab — unchanged behavior, now logged)

- [ ] **5a.** Row 3 on: log shows "Improved subject cultural view with X"; subject's culture view rises; small liberty desire bump; subject then sits on the **50-year cooldown** (not re-hit next month). ⚠️ Report "fires every month" or "never fires once".
- [ ] **5b.** **Skip Subject Culture Leaders** ON + Enforce Culture on → culture-leader subjects not converted; OFF → converted as vanilla.
- [ ] **5c.** The base game's own availability notification still appears normally (no duplicate from this mod).

## 6. Budget + alert

- [ ] **6a. Diplomat consumption (critical):** watch free diplomats when curry/gift/culture actions fire — does the count drop by 1 per action? If NOT, tell Claude (an explicit `add_diplomats = -1` gets added).
- [ ] **6b.** Minimum Diplomats Reserve above your free diplomats → all favor actions stop.
- [ ] **6c.** **Notify When Giftable**: yellow alert when a culture leader owes between the curry cap and gift cap and you have gold; clears/dismisses properly.

## 7. Base-mod regression (quick pass)

- [ ] Improve Relations categories (all 11), limits sliders, block/allow, enforce religion/culture all behave as vanilla base-mod.
- [ ] No `atd`-related error.log spam after a few game-years.

---

**Reporting back:** which checkbox, expected vs. actual, plus error.log snippets.
Code-deciding checks: **2c (favor direction), 3a (gift scriptability), 4a (favor
cost), 5a (cooldown scope), 6a (diplomat consumption)**.

> Remember: delete this file before opening the upstream pull request.
