# Test Checklist — Autonomous Diplomats Enhanced on EU5 1.4 Río Salado (open beta)

> **TEMPORARY** — delete this file before opening any pull request to Conner.

This build is **Conner's Autonomous Diplomats v1.5** plus the features you chose
to keep:

- **Cultural View Reminders**: an alert and window listing the countries whose
  culture's view of yours you can improve right now.
- **Improve Cultural View** for subjects.
- **Enforce Culture: Skip Subject Culture Leaders**.

In the launcher it's called **Autonomous Diplomats Enhanced**.

**Gone:** Curry Favors, Culture Improve, the Favors tab, Automation Priority,
Auto Ask Nobility, and Gift Reminders. Conner's Auto-Gifting now sends the
gifts, so Cultural View Reminders took Gift Reminders' place.

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

## 2. Cultural View Reminders (Auto Interactions tab → Cultural View Reminders)

A country is listed when **all** of these are true. They are the game's own
rules for its **Improve Cultural View** diplomatic action:
- you lead your own culture;
- the country leads *its* culture and isn't your subject (subjects use Subject
  Actions row 3 instead);
- its culture doesn't already see yours as **kindred**;
- you have **at least 50 favors** with it, which is what the action costs.

**Getting 50 favors fast for testing.** Auto-Gifting builds them, but slowly.
In the console you can try
`effect c:FRA = { add_favors = { target = c:TUN value = 50 } }`, replacing FRA
with a country that leads its culture and TUN with your own tag. This syntax
is unverified: if the console rejects it, tell Claude.

- [ ] **2a.** The group has **Notify When a Cultural View Can Be Improved** (on
  by default) with a tooltip.
- [ ] **2b.** With nobody qualifying there's no alert. The **Cultural Views**
  action-bar button (culture icon) opens the window with its "No country
  qualifies right now" text.
- [ ] **2c.** Once a country qualifies, a **green "Cultural View Ready"** alert
  names it at the next month. To see it immediately, turn the setting off and
  on again.
- [ ] **2d.** Left-click the alert. The **Cultural Views You Can Improve**
  window opens with a title, help text and the list. *This window has never
  rendered correctly in-game yet, so this is the most important check here.*
- [ ] **2e. Favor direction.** Click a listed country to open its diplomacy
  screen. Under **Friendly Actions**, **Improve Cultural View** should be
  usable, not greyed out for a lack of favors. If it's greyed out, tell
  Claude.
- [ ] **2f.** Use it. That spends 50 favors, and its culture's view of yours
  goes up one step. The country leaves the list, unless you still have 50+
  favors with it.
- [ ] **2g.** A long list scrolls inside the window. The **X** button closes
  it, and the action-bar button opens and closes it too.
- [ ] **2h.** The alert stays after a left-click. A right-click dismisses it
  until next month.
- [ ] **2i.** Turning the setting off clears the alert straight away. Turning
  Settings → **Enabled** off clears it at the next month and closes the
  window.

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
- **2d** — the Cultural Views window shows its list;
- **2e** — favor direction;
- **3c** — cooldown scope.

Please report on those even if everything else is fine.

> Remember: delete this file before opening a pull request to Conner.
