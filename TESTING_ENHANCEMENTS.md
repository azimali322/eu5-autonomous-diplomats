# Test Checklist — Autonomous Diplomats Enhanced on EU5 1.4 Río Salado (open beta)

> **TEMPORARY** — delete this file before opening any pull request to Conner.

This build is **Conner's Autonomous Diplomats v1.5** plus the features you chose
to keep:

- **Gift Reminders**: the giftable-countries alert and window.
- **Improve Cultural View** for subjects.
- **Enforce Culture: Skip Subject Culture Leaders**.

In the launcher it's called **Autonomous Diplomats Enhanced**.

**Gone:** Curry Favors, Culture Improve, the Favors tab, Automation Priority and
Auto Ask Nobility.

**Changed since July:** Improve Cultural View now follows the game's rule of
**one use every 50 years for your whole country**. In July it could fire on
every eligible subject. See check 3c.

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
  1. **Community Mod Framework - 1.4 Río Salado Dev**. This is the CMF dev
     branch, a local mod in your mod folder.
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

## 2. Gift Reminders (Auto Interactions tab → Gift Reminders)

- [ ] **2a.** The group has **Notify When Giftable** (off by default) and
  **Stop Listing Gifts At** (50), both with tooltips.
- [ ] **2b.** Turn Notify on. A yellow **Gift(s) Available** alert appears
  (right away or at the next month) and names up to 3 countries. They should
  only be:
  - dominant countries of a primary culture;
  - in diplomatic range;
  - not gifted by you in the last 10 years.
- [ ] **2c.** Left-click the alert. The **Countries You Can Gift** window
  opens with a title, help text and the list. *In July it came up blank, so
  this is the most important check here.*
- [ ] **2d.** A long list scrolls inside the window, and the window doesn't
  run off the screen.
- [ ] **2e.** Clicking a country opens its diplomacy screen. From there,
  choose the Economy filter, then Send Gift.
- [ ] **2f.** The **X** button closes the window. The **Giftable Countries**
  action-bar button (favors icon) also opens and closes it.
- [ ] **2g.** The alert stays after a left-click. A right-click dismisses it
  until next month.
- [ ] **2h.** Send a gift to a listed country. It leaves the list (reopen the
  window, or wait a month).
- [ ] **2i.** Queue a listed country with **Start Auto-Gifting**. It leaves the
  list, because Conner's Auto-Gifting now handles it.
- [ ] **2j. Favor direction (never verified before).** Pick a listed country
  and note how many favors it owes you (say 12).
  - With **Stop Listing Gifts At** below that (e.g. 10), it should disappear.
  - With the slider above it (e.g. 15), it should be listed.
  - If it behaves the other way round, tell Claude: the favor direction would
    be inverted.
- [ ] **2k.** Turn Settings → **Enabled** off. At the next month the alert
  disappears and the window closes.

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

---

**Checks that decide code changes:**
- **1a** — the merged outliner;
- **2c** — the gift window shows its list;
- **2j** — favor direction;
- **3c** — cooldown scope.

Please report on those even if everything else is fine.

> Remember: delete this file before opening a pull request to Conner.
