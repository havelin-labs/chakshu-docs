---
icon: material/scale-balance
---

# Tokens and return on spend

## Tokens

Each review reports the tokens it used: input, output, cache reads and cache writes. The **Cost** view shows them per model next to spend. Larger diffs show up as input tokens; the repository context a reviewer reads again on later turns shows up as cache reads, which cost much less. Reviews from runners older than this release report no tokens and count as zero.

## Return on spend

The **Return on spend** panel on the Cost view puts a rough value on the findings that got fixed. Two parts of it are measured and one is your assumption:

- **Measured:** findings fixed in the last 30 days, by severity, and spend in the same period.
- **Assumed:** how many minutes of review work a fixed finding saves at each severity, and what an hour costs.

From those it shows hours saved, their value, and that value minus spend. Until you set an hourly rate it shows only the measured numbers.

The default minutes are 60 for critical, 30 for high, 15 for medium, 5 for low and 0 for info. Change them to what your team thinks a catch is worth. No study gives one right number, so read the result as a guide, not a measurement. Changes to the assumptions go to the [audit log](privacy.md#audit-log).
