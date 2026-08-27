# Voyager copy design

Date: 2026-08-27

## Goal

Add an existing, working travel-planning skill to this repository so trip costs can be calculated consistently across transport, accommodation, food, local transport, activities, rentals, and other expenses.

## Upstream

Source: `HanChan831/voyager` (MIT License).

The upstream repository provides a `voyager/` skill directory containing `SKILL.md`, `budget_calculator.py`, itinerary/timeline helpers, map/photo/reference files, and other supporting assets.

## Chosen approach

Copy the upstream `voyager/` directory substantially as-is into this repository instead of rewriting only the budget logic.

Reasons:
- The requested budgeting workflow already exists and is integrated with the rest of the skill.
- Keeping the upstream structure avoids accidental breakage from partial extraction.
- Future travel-planning features such as itinerary checks and HTML generation remain available.
- The upstream is MIT licensed, so copying and modifying is permitted provided the copyright and permission notice are retained.

## Files to add

1. `voyager/` — copied from `HanChan831/voyager/voyager/`.
2. `LICENSE` — retain the upstream MIT notice when copying substantial portions. If this repository later uses its own project-wide license, preserve the upstream notice in a dedicated third-party notice or within the copied subtree.
3. `README.md` — extend the current README to state that the repository includes/adapts Voyager from `HanChan831/voyager` under the MIT License and briefly explain how to use the skill.

## Behavior to preserve

The copied skill should preserve these upstream properties:
- Store trip facts and costs in structured trip data rather than relying on ad-hoc mental arithmetic.
- Distinguish per-person prices from whole-room / whole-vehicle prices.
- Calculate totals by day and category and provide whole-trip and per-person totals.
- Treat current prices, exchange rates, transport schedules, weather, and similar facts as data to verify rather than invent.
- Support multiple intercity transport modes rather than assuming flights only.

## Initial scope

No Thailand-specific behavioral rewrite is required in the first copy. The first milestone is a faithful, usable copy of Voyager. Thailand-specific defaults or adapters can be added later after the copied skill is verified.

## Attribution and licensing

The upstream MIT copyright and permission notice must be preserved with the copied software. The README should include a short attribution such as:

`Includes/adapts Voyager by HanChan831, licensed under the MIT License.`

## Verification

After copying:
1. Confirm the expected upstream files exist under `voyager/`.
2. Confirm `SKILL.md` references supporting files that were also copied.
3. Run a minimal `budget_calculator.py` smoke test using a small Thailand trip fixture with two travelers, including at least one per-person transport cost and one whole-room accommodation cost.
4. Verify the computed grand total and per-person total are arithmetically correct.
5. Confirm the repository still contains the required MIT notice and attribution.

## Non-goals for this change

- Rewriting Voyager from scratch.
- Removing non-budget Voyager features.
- Adding Thailand-specific hard-coded prices.
- Building a new UI or application around the skill.
