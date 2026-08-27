# Voyager Skill Import Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Import the complete upstream `HanChan831/voyager/voyager/` skill into this repository, preserve MIT attribution, document usage, and verify budget calculation with a two-traveler Thailand smoke test.

**Architecture:** Keep the upstream Voyager subtree intact under `voyager/` rather than extracting only budget logic. Preserve byte-identical upstream files where possible, add the upstream MIT license at repository root, and extend the project README with attribution and basic usage. Verification checks the imported file set/content against the upstream Git tree and runs `budget_calculator.py` on a deterministic Thailand fixture.

**Tech Stack:** Markdown, Python 3, JSON, Git/GitHub.

**Spec:** `docs/superpowers/specs/2026-08-27-voyager-copy-design.md`

## Global Constraints

- Copy the upstream `voyager/` directory substantially as-is.
- Preserve the upstream MIT copyright and permission notice.
- Do not add Thailand-specific hard-coded prices in this change.
- Preserve structured cost accounting, including `per_person` versus whole-room / whole-vehicle prices.
- Verify current facts externally at runtime rather than inventing prices, schedules, exchange rates, or weather.
- Do not remove non-budget Voyager features.

---

### Task 1: Import the upstream Voyager subtree

**Files:**
- Create: `voyager/SKILL.md`
- Create: `voyager/budget_calculator.py`
- Create: `voyager/calendar_notes.md`
- Create: `voyager/customs_guidance.md`
- Create: `voyager/geocode_lookup.py`
- Create: `voyager/html_content_guide.md`
- Create: `voyager/itinerary_html_builder.py`
- Create: `voyager/local_needs_checklist.md`
- Create: `voyager/map_link_reference.md`
- Create: `voyager/online_map_embed.md`
- Create: `voyager/ota_platforms.md`
- Create: `voyager/photo_lookup.md`
- Create: `voyager/photo_lookup.py`
- Create: `voyager/pretrip_checklist.md`
- Create: `voyager/ratings_guidance.md`
- Create: `voyager/schedule_checker.py`
- Create: `voyager/services_guidance.md`
- Create: `voyager/sharing_guide.md`
- Create: `voyager/trip_template.html`
- Create: `voyager/visa_checklist.md`
- Create: `voyager/weather_guidance.md`

**Interfaces:**
- Consumes: upstream tree `HanChan831/voyager` at `voyager/`.
- Produces: a complete local Voyager skill subtree with the same 21 file paths and contents.

- [ ] **Step 1: Record the upstream recursive tree**

Use GitHub's recursive tree endpoint for upstream tree SHA `26cc101446d9f98df1fa531e9c364304ee075031` and confirm `truncated=false` with exactly 21 blobs.

- [ ] **Step 2: Copy all 21 upstream blobs into `voyager/`**

Create each destination path with the exact upstream UTF-8 content. Do not edit the imported files during this task.

- [ ] **Step 3: Verify the destination tree**

Fetch the destination recursive tree for `voyager/` and compare path set plus blob SHA against the upstream tree. Expected result: 21/21 paths present and matching content SHAs.

- [ ] **Step 4: Commit**

Commit message: `feat: import Voyager travel planning skill`

### Task 2: Preserve license and attribution

**Files:**
- Create: `LICENSE`
- Modify: `README.md`

**Interfaces:**
- Consumes: upstream MIT license text and imported `voyager/` skill.
- Produces: repository-level license notice and user-facing attribution/usage notes.

- [ ] **Step 1: Add the upstream MIT license**

Create `LICENSE` with the exact upstream MIT text, including `Copyright (c) 2026 寒江在下雪`.

- [ ] **Step 2: Extend README**

Replace the minimal README with:

```markdown
# thai-travel-plan-creator-

Travel-planning skill workspace with structured trip budgeting and itinerary support.

## Voyager skill

This repository includes/adapts [Voyager](https://github.com/HanChan831/voyager) by HanChan831, licensed under the MIT License.

The imported skill lives in `voyager/`. Its budgeting workflow stores trip costs as structured items and distinguishes per-person costs from whole-room / whole-vehicle costs before calculating day, category, trip, and per-person totals.

### Budget calculator

```bash
python3 voyager/budget_calculator.py trip_data.json budget_summary.md
```

See `voyager/SKILL.md` for the full travel-planning workflow.
```

- [ ] **Step 3: Verify attribution**

Confirm `LICENSE` contains `MIT License` and the upstream copyright line, and README names `HanChan831/voyager` plus MIT.

- [ ] **Step 4: Commit**

Commit message: `docs: add Voyager license and attribution`

### Task 3: Run a deterministic Thailand budget smoke test

**Files:**
- Test fixture: temporary `trip_data.json` (do not commit unless needed for debugging)
- Test output: temporary `budget_summary.md` (do not commit)

**Interfaces:**
- Consumes: `voyager/budget_calculator.py`.
- Produces: verified arithmetic for two travelers with mixed per-person and shared costs.

- [ ] **Step 1: Create the smoke-test fixture**

Use exactly this JSON:

```json
{
  "trip": {
    "origin": "Bangkok",
    "destination": "Krabi",
    "start_date": "2026-08-27",
    "travelers": 2,
    "currency": "THB"
  },
  "items": [
    {
      "day": 1,
      "category": "長途交通",
      "description": "Bangkok to Krabi overnight bus",
      "amount": 749,
      "per_person": true
    },
    {
      "day": 2,
      "category": "住宿",
      "description": "Private room",
      "amount": 600,
      "per_person": false
    },
    {
      "day": 2,
      "category": "当地交通",
      "description": "Motorbike rental",
      "amount": 250,
      "per_person": false
    }
  ]
}
```

Expected total: `749*2 + 600 + 250 = 2348 THB`; expected per-person total: `1174 THB`.

- [ ] **Step 2: Run the budget calculator**

Run:

```bash
python3 voyager/budget_calculator.py /tmp/thai-trip-data.json /tmp/budget_summary.md
```

Expected stdout contains `总预算：2348.00 THB` and `人均 1174.00 THB`.

- [ ] **Step 3: Inspect generated Markdown**

Confirm `/tmp/budget_summary.md` contains the route, 2 travelers, three item rows, grand total `2348.00 THB`, and per-person total `1174.00 THB`.

- [ ] **Step 4: Verify Python syntax for executable helpers**

Run:

```bash
python3 -m py_compile voyager/budget_calculator.py voyager/geocode_lookup.py voyager/itinerary_html_builder.py voyager/photo_lookup.py voyager/schedule_checker.py
```

Expected: exit code 0 and no syntax errors.

### Task 4: Final repository verification and PR

**Files:**
- Review: all files added/modified on `feat/import-voyager-skill`.

**Interfaces:**
- Consumes: Tasks 1–3.
- Produces: reviewable pull request against `main`.

- [ ] **Step 1: Compare branch to main**

Confirm changes contain only the imported Voyager subtree, license/README attribution, design spec, and implementation plan.

- [ ] **Step 2: Re-run content checks**

Confirm all 21 upstream Voyager paths exist and match upstream blob SHAs; confirm license and attribution are present.

- [ ] **Step 3: Open pull request**

Title: `Import Voyager travel planning skill`

Body must summarize the 21-file upstream import, MIT attribution, and smoke-test result (`2348 THB total / 1174 THB per person`).
