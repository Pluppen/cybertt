# TTX Platform — Facilitator Guide

A browser-based tool for running cybersecurity incident response tabletop exercises.
No installation required — open `index.html` in any modern browser.

---

## What is a tabletop exercise?

A tabletop exercise (TTX) is a structured discussion-based activity where a small team (typically 2–6 people) works through a simulated security incident in real time. There is no live infrastructure involved — the value comes from the conversation: who makes decisions, how quickly, with what information, and against what constraints.

The facilitator's job is to drive the scenario forward, introduce new developments (called **injects**), track what the team decides, and note where they struggle. This tool handles the mechanics so the facilitator can stay focused on the room.

---

## Overview

The application has four views:

| View | Purpose |
|---|---|
| **Scenario Library** | Browse, import, export, and launch scenarios |
| **Scenario Builder** | Create or edit scenarios and their injects |
| **Live Session** | Run an active exercise |
| **Debrief** | Review the session timeline and decision log |

All data is stored locally in the browser (`localStorage`). Nothing is sent to any server. The app works fully offline.

---

## Scenario Library

The home screen. Each card shows the scenario title, incident type, inject count, and role count.

**Per-card buttons:**
- **EXPORT** — downloads the scenario as a `.json` file. Use this to back up scenarios or share them with other facilitators.
- **EDIT** — opens the scenario in the builder
- **START** — launches a live session using that scenario

**Header buttons:**
- **IMPORT JSON** — opens a file picker to load a previously exported `.json` scenario file. If the imported scenario has an ID that conflicts with an existing one, a new ID is generated automatically so nothing is overwritten.
- **+ NEW SCENARIO** — creates a blank scenario in the builder

Two scenarios are included by default:

| Scenario | Type | Injects |
|---|---|---|
| Ransomware Outbreak | Ransomware | 6 |
| Phishing-Led Breach | Phishing | 5 |

---

## Running a Live Session

This is the primary view. It has two panels.

### Top bar

**Real elapsed timer** — shows how long the session has been running in `MM:SS` format. Use this to pace the exercise.

**Scenario clock** — shows the simulated time of day within the incident scenario (e.g. `09:47`). Time advances at 12× real speed: **5 real minutes = 1 scenario hour**. This ratio is set so a 90-minute session covers roughly an 18-hour incident window.

The scenario clock starts at the time embedded in the scenario's situation brief (e.g. `08:47` for the ransomware scenario). Every event in the log is timestamped in scenario time, so the team can reference events naturally — "what did we decide at 10:30?" — without converting from raw elapsed seconds.

The `5 min = 1 hr` ratio marker is shown between the two clocks as a constant reminder.

**Situation Brief** — opens the full opening brief and role descriptions in a modal. Useful if a participant asks to be reminded of the starting conditions.

### Left panel — facilitator controls

**Tension Meter**
A 0–10 slider representing the pressure level in the room. Drag it up as the scenario escalates. The color shifts automatically:

- 0–3: green (nominal)
- 4–6: yellow (elevated)
- 7–10: red (critical)

Adjusting the tension logs an event in the feed after a short delay, so rapid small adjustments don't flood the log.

**Current Inject card**
The inject the team is currently working through. Shows the title, scenario development text, and a discussion prompt. The discussion prompt is a guiding question for the facilitator — it does not need to be read verbatim.

**Dice Roller**
Appears on injects that have a dice table configured. Roll the D6 to introduce an element of uncertainty into the outcome. The result maps to a predefined outcome range and is logged automatically. You can roll multiple times on the same inject before firing it — only logged rolls appear in the feed.

**FIRE INJECT**
Marks the current inject as delivered, logs it to the event feed, and advances to the next inject. Use this when the team has finished discussing and you are ready to move forward.

**SKIP**
Advances to the next inject without logging. Use this if an inject is no longer relevant given how the scenario has developed, or if time is short.

**LOG DECISION**
Opens a text prompt to record what the team decided. This is the most important thing to capture during an exercise — not just what happened, but what the team *chose* to do. Decision entries appear prominently in the debrief. Use Cmd/Ctrl+Enter to submit quickly.

**Up Next**
A compact list of upcoming injects. Useful for pacing — if you are running short on time, you can see what is left and decide which injects to skip.

### Right panel — event feed

A live reverse-chronological log of everything that has happened in the session. Each entry shows:

- An icon and label indicating the event type (inject fired, dice roll, decision, tension change)
- The scenario time when the event occurred (e.g. `10:23`)
- The relevant detail text

The event feed is read-only during the session. It becomes the source of truth in the debrief.

### Session persistence

If you close or refresh the browser mid-session, the session is automatically restored exactly where you left it when you reopen `index.html`. The timer restarts from the correct elapsed time based on the original start timestamp.

---

## Debrief

Displayed immediately after ending a session.

**Stats bar** — shows total duration, injects fired, decisions logged, and dice rolls.

**Full Timeline** — every event in chronological order with scenario timestamps. This is the complete record of what happened.

**Decision Log** — all logged decisions in order, with scenario timestamps. Extracted separately so they are easy to discuss as a group without scrolling through the full timeline.

**Export Text** — downloads a `.txt` file containing the full debrief: session metadata, the complete timeline, and the decision log. All timestamps in the export use scenario time. The filename includes the scenario name and date.

**New Session / Library** — start another run with the same scenario, or return to the library.

---

## Scenario Builder

Used to create new scenarios or modify existing ones. Changes are saved to `localStorage` when you click **SAVE SCENARIO**.

### Fields

**Title** — the name shown on the library card and throughout the session.

**Incident Type** — a short label (e.g. `Ransomware`, `Phishing`, `Supply Chain`).

**Scenario Start Time** — the time of day where the scenario clock begins when a session starts. Set this to match the time mentioned in your situation brief. Defaults to `08:00`. The clock advances at 12× real speed from this point.

**Short Description** — one or two sentences shown on the library card.

**Situation Brief** — the opening narrative read to all participants at the start of the exercise. Write this in the present tense, as if the incident is unfolding now. Include enough specifics to make it feel real — specific times, system names, and numbers help.

### Roles

Each role has three fields:

- **Name** — the title (e.g. `CISO`, `SOC Lead`)
- **Description** — the responsibilities and scope of this role in the exercise
- **Initial information** — private context given to this participant at the start, before the exercise begins. Use this to create information asymmetry — each role should know something the others don't.

### Injects

Injects are the scenario developments delivered by the facilitator during the exercise. They are delivered in order, one at a time.

**Reordering** — use the up/down arrows on each inject card to change the sequence. The order numbers update automatically.

**Title** — a short label shown in the current inject card and the event log.

**Description** — the text read or paraphrased to participants when the inject fires. Be specific. Vague injects produce vague discussions.

**Discussion Prompt** — a guiding question for the facilitator. Not visible to participants. Good discussion prompts ask *who decides*, *what are the trade-offs*, or *what do you not yet know*.

**Dice Table** — enable this to add a dice roll to the inject. Add rows with a range (e.g. `1-2`, `3-4`, `5-6`) and an outcome description for each range. The outcomes should represent meaningfully different scenario branches — don't use dice just for randomness, use them when the variance actually changes how the team should respond.

---

## Sharing and portability

Scenarios are stored in `localStorage`, which is browser- and device-specific. To move a scenario to another machine or share it with a co-facilitator:

1. Click **EXPORT** on the library card — this downloads a `.json` file.
2. On the other machine, click **IMPORT JSON** in the library header and select the file.

The import validates that the file contains the required fields and normalises any missing ones to safe defaults.

---

## Tips for facilitators

**Before the session**
- Assign roles before participants arrive. Give each person their role card (name, description, and initial information) privately so they have time to read it.
- Read the situation brief aloud at the start. Don't distribute it as a handout — hearing it creates urgency.
- Explain the rules once: no right answers, decisions are made with imperfect information, the goal is to surface gaps not test knowledge.
- Check the scenario clock start time matches the time mentioned in the brief.

**During the session**
- Don't fire injects too quickly. The most valuable moments are the conversations that happen between injects. Let silence do work.
- Use the discussion prompt as a recovery tool when the group gets stuck or goes too deep into technical detail.
- Log decisions in real time. If you wait until after, you will miss or misquote them. A good decision log entry captures the *what* and the *who* — "Team decided to isolate the finance VLAN. IT Ops owner. CISO informed."
- Raise the tension meter when the team is handling things well. It signals that the scenario is about to get harder and keeps the energy up.
- Roll dice at dramatically useful moments — after the team has committed to a plan that depends on the outcome.
- Reference scenario time when discussing events: "at 10:30 we decided to..." keeps the conversation anchored to the incident timeline rather than the clock on the wall.

**After the session**
- Run the debrief immediately while the session is fresh. Don't schedule it for later.
- Export the debrief and share it with participants within 24 hours while it is still meaningful.

**On scenario design**
- Six to eight injects is the right range for a 90-minute session. More than that and you are rushing; fewer and you may run short.
- The best injects introduce a complication to a decision the team has already made, not a new isolated problem. If the team decided to call the IR vendor in inject 2, inject 4 should test that decision.
- At least one inject should create pressure for an external communication the team is not ready to make. Press inquiries, regulator contact, and customer questions expose the most useful gaps.
- Put your hardest inject last. Ending on a resolved note wastes the exercise.
- When designing the scenario timeline, work backwards from the scenario start time. If the scenario opens at 08:47 and a 90-minute session covers ~18 scenario hours, your final inject should land around the late afternoon or evening of the same day.
