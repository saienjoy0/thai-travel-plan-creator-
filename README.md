# thai-travel-plan-creator-

Travel-planning skill workspace with structured trip budgeting and itinerary support.

## Voyager skill

This repository includes/adapts [Voyager](https://github.com/HanChan831/voyager) by HanChan831, licensed under the MIT License.

The imported skill lives in `voyager/`. Its budgeting workflow stores trip costs as structured items and distinguishes per-person costs from whole-room / whole-vehicle costs before calculating day, category, trip, and per-person totals.

Imported from upstream commit `38e33e4044a18500d0df99d3ddda1681ef1c8657`.

### Budget calculator

```bash
python3 voyager/budget_calculator.py trip_data.json budget_summary.md
```

See `voyager/SKILL.md` for the full travel-planning workflow.
