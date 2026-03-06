# AI Cost Estimator — JSON Output Spec

Use this spec when the user requests JSON output for the web tool, API, or data export. Return ONLY valid JSON — no markdown, no backticks, no explanation.

## JSON Schema

```json
{
  "project_name": "string — short name for the project",
  "project_type": "software | physical | hybrid",
  "completion_pct": 0-100,
  "summary_line": "string — one punchy sentence describing what was built",
  "pre_ai": {
    "total_cost": 123456,
    "total_hours": 1234,
    "calendar_months": 12,
    "team_size": 5,
    "line_items": [
      {
        "label": "Component name",
        "hours": 100,
        "cost": 12500,
        "rate_basis": "$125/hr senior full-stack dev"
      }
    ]
  },
  "post_ai": {
    "total_cost": 1234,
    "total_hours": 12,
    "calendar_months": 1,
    "team_size": 1,
    "line_items": [
      {
        "label": "Component name",
        "hours": 2,
        "cost": 40,
        "rate_basis": "Claude Pro subscription prorated"
      }
    ]
  },
  "forecast_full": {
    "enabled": true,
    "pre_ai_total": 234567,
    "post_ai_total": 2345,
    "pre_ai_months": 18,
    "post_ai_months": 3
  },
  "reductions": {
    "cost_pct": 98,
    "time_pct": 91,
    "team_pct": 80
  },
  "speed_multiplier": 69,
  "roi": 4868,
  "hero_stat": "$257,947 saved",
  "hook_line": "What took a team of 8 two years now took one person 30 hours."
}
```

## Schema Rules

- `forecast_full.enabled` = true only when `completion_pct` < 100
- `reductions` percentages are integers (0–100): `round((pre - post) / pre * 100)`
- `speed_multiplier` = `round(pre_ai.total_hours / post_ai.total_hours)`
- `roi` = `round((pre_ai.total_cost - post_ai.total_cost) / post_ai.total_cost)`
- `hero_stat` = formatted string of net savings (e.g. "$257,947 saved")
- `hook_line` = one sentence, loss-framed ("What would have cost…" or "What took…")
- All cost values are integers (no decimals)
- `line_items` must have at least 3 items per section; aim for 6–12 for pre-AI
- Every line item must have all 4 fields populated: label, hours, cost, rate_basis
