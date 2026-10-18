# Test Checklist — Autonomous Diplomats Enhanced on EU5 1.4.1 Río Salado (open beta)

> **TEMPORARY** — delete this file before opening any pull request to Conner.

This build is **Conner's Autonomous Diplomats v1.5** plus:

- **Cultural Views** (Auto Interactions tab), which is new for 1.4.1:
  - **Curry Favors for Cultural Views** curries favors with countries that lead
    their culture until you have 50.
  - **Auto Improve Cultural View** spends those 50 favors on Improve Cultural
    View the day they arrive.
  - **Notifications** cover both: you can improve one, or the mod just did.
- **Improve Cultural View** for subjects.
- **Enforce Culture: Skip Subject Culture Leaders**.
- **Automation Priority** (Settings tab): a draggable list that sets which
  monthly action gets free diplomats first. Auto Improve Cultural View always
  goes before all of them.

In the launcher it's called **Autonomous Diplomats Enhanced**.

**Gone:** the July Favors tab and its curry engine, Culture Improve, Automation
Priority, Auto Ask Nobility, and Gift Reminders (Conner's Auto-Gifting sends
the gifts now).

**Why 1.4.1 matters here:** the patch raised the curry cap to 50, which is
exactly what Improve Cultural View costs. It also added favor decay (1% of your
favors each month), so you lose the 50 within a month of reaching it. And other
countries can now refuse Improve Cultural View.

**Changed since July:** Improve Cultural View now follows the game's rule of
**one use every 50 years for your whole country**. In July it could fire on
every eligible subject. See check 3c.

**Changed on October 18:**
- **Auto Improve Cultural View applies the action itself now.** Asking the
  game to do it failed twice over:
  - as a plain request it never went through. Your 1360.1.1 autosave showed
    16 tries (8 on Kanem, 6 on Mali, 2 on Tunis), each costing a diplomat;
  - with `mode = accept` the game only settled it weeks later, and only if
    your favors were still at 50 by then. They decay from the moment a curry
    tops out, so Mali failed once and succeeded once, and Kanem sat unresolved
    from April 2nd to April 8th, 1363.

  The mod now does what the game's action does, at once: the view goes one
  step up, and it takes the 50 favors and one diplomat. It still only targets
  countries that would accept, and tries each country at most once every two
  months. See 2g.
- **Every automated action keeps both Reserve settings** (Settings tab →
  Reserve), as you asked:
  - Minimum Diplomat Gain now applies to Auto-Gifting and Subject Actions too,
    which Conner had exempted;
  - Auto Improve has its **own** diplomat reserve, the new **Improve Cultural
    View Reserve** in the same group (second October 18 request). It can be
    lower than Minimum Diplomats Reserve but never higher, so a curry that
    reaches 50 is spent even while the other automations hold diplomats back.

  See 2i and 5f.

**Updated for 1.4:** the Community Mod Toolkit's GUI Update Tool merged 1.4's
changes into the game screens Conner's mod overrides:
- the outliner, including its new **expedition** and **trade order** entries;
- the diplomatic-action confirmation popup;
- the automation card;
- the Create Playset button.

Section 1 checks them.

**How to report:** tick boxes and write notes under any item, like you did in
July. Then tell me you're done. I read `error.log`, `game.log` and the Mod
Action Log myself, so you only need screenshots for visual problems.

**Mod Action Log:** mod menu → *Community Mod Framework* (left list) →
*General* → *Session* → *Mod Action Log* → *Open*.

---

## 0. Setup (once)

- [ ] Your mod folder is on this build, branch `update/1.4-rio-salado`.
- [ ] Playset, in this order:
  1. **Community Mod Framework Dev** (v2.5.0). This is the CMF dev branch, a
     local mod in your mod folder; it was called "Community Mod Framework -
     1.4 Río Salado Dev" before this update.
  2. **Autonomous Diplomats Enhanced**.
- [ ] Turn **off**:
  - the Workshop "Community Mod Framework - 1.3 Pavia" (two copies of CMF
    conflict);
  - the Workshop "Autonomous Diplomats" (it conflicts with this build);
  - Cooldown Notifier (it adds noise).
- [ ] The launcher shows no missing-dependency or outdated warning for
  Autonomous Diplomats Enhanced.
- [ ] Start a **new game**. 1.3 saves aren't guaranteed to work in 1.4. Tunis
  is fine.
- [ ] Optional, but it lets Claude check every identifier against 1.4's
  official list: open the console and run `debug_mode`, then `script_docs`,
  then `dump_data_types`.

## 1. Conner's v1.5 on 1.4 (baseline + the 1.4 screen updates)

This is the first time v1.5 has run on 1.4. Claude merged 1.4's screen changes
into Conner's overrides, so report anything odd here.

- [ ] **1a. Outliner** (the right-hand panel):
  - its sections (armies, constructions, cabinet, diplomacy, markets…) look
    normal and open their windows;
  - start an **expedition** or create a **trade order** — it gets an outliner
    entry. Before this fix, Conner's outliner had no entries for these 1.4
    features.
- [ ] **1b. Diplomatic confirmation popups.** Manual diplomatic actions still
  ask for confirmation as usual, and the accept icon's tooltip appears.
- [ ] **1c. Automation card** (Diplomacy Interactions):
  - its Improve Opinion checkbox turns this mod's Improve Relations on and off;
  - **new in 1.4:** the checkbox is greyed out while the Diplomacy Interactions
    automation itself is switched off.
- [ ] **1d. Main menu → Mods/playsets.** **Create Playset** now opens a name box
  on the first click (1.4 behaviour), and creating a playset still works. No
  "missing Community Mod Framework" popup appears.

- [ ] The mod menu opens with these tabs: **Improve Relations, Auto
  Interactions, Subjects, Settings**.
- [ ] Improve Relations: set one category to 1–2 and let a few months pass.
  Relation improvements start.
- [ ] Auto-Gifting:
  1. Open a country's diplomacy screen.
  2. In the **Autonomous Diplomats** category, use **Start Auto-Gifting**. The
     country appears in the Auto Interactions queue.
  3. Once its gift cooldown is ready, a gift goes out: your treasury drops and
     its favors rise.
- [ ] Enforce Religion / Enforce Culture on subjects still work.

## 2. Cultural Views (Auto Interactions tab → Cultural Views)

**Who qualifies.** The mod only works with countries where the whole plan can
succeed. These are the game's own 1.4.1 rules, plus your "no rivals" rule:
- you lead your own culture (Improve Cultural View needs it);
- the country leads *its* culture and isn't your subject (subjects use Subject
  Actions row 3 instead);
- its culture doesn't already see yours as **kindred**;
- **their opinion of you is above 50.** Since 1.4.1 they can refuse: they
  accept only when −10 + 0.2 × opinion is positive. A refusal costs you trust
  and reputation;
- **neither of you is the other's rival**;
- for currying only:
  - they trust you **at least 50**, because favors can only be curried up to
    their trust in you;
  - they aren't anyone's subject (the game won't curry those).

**Where to look:**
- the outliner's Diplomacy section shows each "Currying Favors" in progress;
- the Mod Action Log shows "Started currying favors with X" and "Spent 50
  favors and improved the cultural view of X".

**Speeding it up.** A curry takes years to reach 50. The console may accept
`effect c:FRA = { add_favors = { target = c:TUN value = 49 } }` (FRA = a
country that leads its culture, TUN = your tag). This syntax is unverified: if
it's rejected, tell Claude.

### 2A. The settings
- [ ] **2a.** The **Cultural Views** group has four settings, each with a
  tooltip:
  - **Curry Favors for Cultural Views** (off by default);
  - **Countries to Curry at Once** (2);
  - **Auto Improve Cultural View** (off);
  - **Notify When a Cultural View Can Be Improved** (on).

### 2B. Curry Favors for Cultural Views
- [ ] **2b.** Turn it on. At the next month the Mod Action Log shows "Started
  currying favors with …" for up to 2 countries, and the outliner shows them
  as Currying Favors. Check each one against the rules above: opinion above
  50, trust 50+, not a rival, not a subject, leads its culture. Countries whose
  culture lives in your own provinces should be picked first.
- [ ] **2c. Exactly 2 at once (the July bug).** Let several months pass. There
  are always 2 curries running (or fewer, if fewer countries qualify), with no
  new "Started currying" entries for countries already being curried.
- [ ] **2d.** Cancel one curry yourself (right-click it in the outliner). At
  the next month the mod starts a curry with a *different* country to fill the
  slot. It waits 6 months before retrying the one you cancelled.
- [ ] **2e.** Raise **Countries to Curry at Once** to 3, and a third curry
  starts next month. Set your diplomat reserve higher than your diplomats, and
  no new curries start.
- [ ] **2f. Trust cap.** If a curried country stops gaining favors below 50,
  note its trust in you. Tell Claude: the trust rule then works differently
  than the game text suggests.

### 2C. Auto Improve Cultural View
Auto Improve checks **every day**, which fixed the October 17 report: a curry
stopped at 50, Auto Improve didn't fire, and the 50 decayed away. It keeps your
**Reserve** settings, with a diplomat reserve of its own. It acts only when:
- you have more diplomats than **Improve Cultural View Reserve** (Settings →
  Reserve; it starts at 1). If Minimum Diplomats Reserve is lower, that one is
  used;
- your monthly gain is at least Minimum Diplomat Gain (0.1 in your save).

**Fixed on October 18:** it now applies the action itself (see the note at the
top). The quickest test is your **SP_MOR_1363_04_08** save, where Kanem is at
50.16 favors. Load it on the new build and unpause. Kanem's earlier unanswered
try is cleared once on load, so Auto Improve should improve Kanem within a day
or two.

- [ ] **2g. It really works, the same day.** Turn it on. **On the day** a
  curried country reaches **50 favors**, not at the next month:
  - the Mod Action Log shows "Spent 50 favors and improved the cultural view
    of …";
  - a green **"Cultural View Improved"** alert names it;
  - your favors with it drop by 50, and its culture's view of yours goes up one
    step;
  - you have one diplomat fewer, and only one.

  **This is the most important check.** Tell Claude straight away if the log
  says either of these instead:
  - "Used Improve Cultural View, but the view has not changed yet": the view
    change didn't apply;
  - "The 50 favors were not taken…": the view went up for free.
- [ ] **2h.** No game popup or "accepted" notification appears for it, because
  the mod applies it directly. Its own green alert and log entry are the
  record.
- [ ] **2i. Its own reserve (the Mali case).** On the Settings tab, the
  Reserve group has a third slider, **Improve Cultural View Reserve**, with a
  tooltip. Set Minimum Diplomats Reserve to 4 and Improve Cultural View Reserve
  to 1.
  - With 2 or more diplomats and a country at 50 favors, Auto Improve fires
    within a day, even though the other automations wait for more than 4.
  - With fewer than 2, it waits, and the log says "no diplomat free above your
    Improve Cultural View Reserve".
  - Set it to 0 and it fires with a single free diplomat.
  - Drag it above Minimum Diplomats Reserve: it snaps back to the same value.
    Lower Minimum Diplomats Reserve below it, and Auto Improve uses the lower
    one.
- [ ] **2j. Skips explain themselves.** If you have 50 favors with a country
  and the mod doesn't press it, the Mod Action Log says why within a day:
  - "you are rivals";
  - "they would refuse until their opinion of you is above 50";
  - "no diplomat free above your Improve Cultural View Reserve";
  - "your monthly diplomat gain is below Minimum Diplomat Gain";
  - "favors stuck just under 50 … their trust in you is below 50".

  An attempt that doesn't go through is logged, and that country is not tried
  again for two months. After the two months, the log says either "Spent 50
  favors and improved…" or "did not go through". Your diplomats never drain
  day after day again.

  If none of these appears and it still didn't press, tell Claude. Also hover
  **Improve Cultural View** on that country: does the game say they **will
  accept**? Note their opinion of you. That tells Claude whether the
  "opinion above 50" rule is right.
- [ ] **2k.** Next month the mod starts currying that country again, and the
  cycle repeats until its culture sees yours as kindred. After that it moves on
  to other countries and never curries a kindred one.

### 2D. Notifications and the window
- [ ] **2l.** With **Auto Improve off**, when a country reaches 50 a green
  **"Cultural View Ready"** alert names it. The mod then leaves that country
  uncurried until its favors decay below 45, so it doesn't spend diplomats
  topping it back up while you decide.
- [ ] **2m.** Left-click that alert. The **Cultural Views You Can Improve**
  window opens with a title, help text and the list. *This window has never
  rendered correctly in-game yet.*
- [ ] **2n.** Click a listed country to open its diplomacy screen. **Improve
  Cultural View** under Friendly Actions is usable. If it's greyed out for a
  lack of favors, tell Claude, because the favor direction would be wrong.
- [ ] **2o.** The X button and the **Cultural Views** action-bar button
  (culture icon) both close the window. A right-click dismisses an alert.
  Turning Notify off clears both alerts, and turning Settings → **Enabled** off
  stops everything.

## 3. Improve Cultural View (Subjects tab → Subject Actions, row 3)

It only works when all of these are true:
- you are the dominant country of your own culture;
- a subject is the dominant country of *its* culture;
- that subject has 50+ loyalty and under 50 liberty desire;
- its culture doesn't already see yours as kindred.

- [ ] **3a.** The Subject Actions list has a 3rd row, **Improve Cultural
  View**, with a checkbox and a tooltip. It can be reordered like the other
  two.
- [ ] **3b.** Tick it. Within a month:
  - the Mod Action Log shows *"Improved the cultural view of \<subject\>"*;
  - that culture's view of yours goes up one step;
  - the subject's liberty desire rises a little;
  - you have one fewer diplomat.
- [ ] **3c. Cooldown (important, and changed since July).** After it fires
  once, it should not fire again for 50 years, on any subject.
  - Open *another* eligible subject's diplomacy screen. The game's own
    Improve Cultural View action should show as on cooldown there too.
  - If the game's own action is still available there, tell Claude. That
    would mean the game's cooldown is per subject.
- [ ] **3d.** Untick it and it stops. *New fix:* ticking or unticking takes
  effect without reopening the menu (in July it waited for that).
- [ ] **3e.** With Settings → **Enabled** off, it never fires.

## 4. Enforce Culture: Skip Subject Culture Leaders (Subjects tab → Options)

- [ ] **4a.** Turn on **Enforce Culture** (row 2) and this option. A subject
  that leads its own culture is **not** converted; other subjects still are.
- [ ] **4b.** Turn the option off. Enforce Culture converts that subject as
  normal.

## 5. Automation Priority (Settings tab)

Each month the mod hands out free diplomats in this order. Priority only
matters when there aren't enough diplomats for everything. It never cancels
actions already running.

1. **Auto Improve Cultural View**: always first, so it isn't in the list.
2. The **Automation Priority** list, top to bottom.

- [ ] **5a.** The Settings tab has an **Automation Priority** list with a
  tooltip. Its four rows can be dragged, and by default they read: **Subject
  Actions, Auto-Gifting, Curry Favors for Cultural Views, Improve Relations**.
  The header shows "Automation Priority", not a raw key like
  `atd__settings__enh_priority_name`.
- [ ] **5b. The order is followed.** The easiest test is to make diplomats
  scarce:
  1. Raise Settings → Minimum Diplomats Reserve until only about one diplomat
     a month is free.
  2. Turn on Curry Favors for Cultural Views and an Improve Relations category
     that has targets.
  3. With Curry above Improve Relations, the free diplomat starts a curry
     ("Started currying favors with …" in the Mod Action Log).
  4. Drag **Improve Relations** above **Curry**. From the next month the free
     diplomat starts an Improve Relations instead.
- [ ] **5c.** With a country at 50 favors and Auto Improve on, it gets the
  free diplomat ahead of every row in the list.
- [ ] **5d. Nothing runs twice.** Improve Relations never goes over its
  category limits, and Enforce Culture/Religion fire once per their 10-year
  cooldown, as before.
- [ ] **5e.** The order you set survives a save and reload.
- [ ] **5f. The Reserve settings apply to everything.** Raise Settings →
  **Minimum Diplomat Gain** above your current monthly diplomat gain. No new
  action of any kind starts:
  - Improve Relations;
  - Curry Favors for Cultural Views;
  - Auto Improve Cultural View;
  - Subject Actions;
  - Auto-Gifting, whose queue shows "Waiting for diplomats".

  Do the same with **Minimum Diplomats Reserve** at or above your banked
  diplomats: nothing starts except Auto Improve, which follows its own
  Improve Cultural View Reserve (2i). Lower them again and everything resumes.

---

**Checks that decide code changes:**
- **2c** — exactly the set number of curries at once (the July bug);
- **2g** — Auto Improve fires the same day favors reach 50, spends them, and
  raises the view (the October 18 fix);
- **2i** and **5f** — the Reserve settings hold for every action;
- **2j** — every skip is explained in the Mod Action Log;
- **2m** and **2n** — the window shows its list, and the favor direction is
  right;
- **1a** — the merged outliner (the curry tracker lives in it);
- **3c** — subject cooldown scope;
- **5b** and **5d** — the priority order is followed, and nothing runs twice.

Please report on those even if everything else is fine.

> Remember: delete this file before opening a pull request to Conner.
