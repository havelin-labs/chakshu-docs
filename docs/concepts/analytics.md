---
icon: material/chart-box-outline
---

# Analytics

**Analytics** shows admins how Chakshu is used across the organization and what it costs. **My activity** shows each person their own usage.

Every view takes a period (7, 30, 90 or 180 days, or a year) and can be filtered by repository, reviewer, trigger and team. The filters live in the page address, so you can share a view by sharing its link. Times are UTC.

## Views

| View | What it shows |
|---|---|
| Overview | Reviews, pull requests reviewed, spend, cost per fixed finding, findings posted and fixed, fix rate and the share of reviews someone asked for, each against the previous period of the same length. Daily reviews and daily spend. |
| Repositories, Reviewers, Skills | For each: reviews, PRs, spend, findings posted, fixed, dismissed and open, fix rate and cost per fixed finding. Export to CSV. Pick a reviewer to filter everything by it, or a skill to see its findings. |
| Teams | The same numbers per GitHub team. See [Teams](teams.md). |
| Review speed | Time to the first human review and to merge, for PRs Chakshu reviewed and the rest. See [Review speed](review-speed.md). |
| People | Every member of the organization as a card, with a page per person. See [People](people.md). |
| Cost | This month's spend against the monthly cap, a straight-line projection, the day the cap would be reached at the current rate, reviews stopped by the per-review cap, tokens by model, spend by repository, reviewer, model and trigger, and an estimate of the return on spend. See [Tokens and return on spend](return-on-spend.md). |
| Findings | The newest 200 findings with their outcome and how it was decided. |

Admins can also have Chakshu post to Slack as spend nears the cap, plus a weekly digest. See [Spend alerts](spend-alerts.md).

## Definitions

- **Review:** a finished run that wasn't an answer to a [reply](replies.md).
- **Spend:** the cost of every run in the period, failed ones included, because they were paid for.
- **Fix rate:** fixed ÷ (fixed + dismissed). Findings still open have no verdict and are left out.
- **Fixed / dismissed:** a finding is fixed when the code under Chakshu's comment changes, and dismissed when someone resolves the thread without changing the code. Either can also be decided when the reviewer answers a reply. The Findings view says which happened.
- **Asked for:** reviews someone requested (a mention, a review request, the portal or a reply), as opposed to automatic reviews.
- **Cost per fixed finding:** spend ÷ findings fixed in the period.

## People and privacy

Numbers about people are limited on purpose: usage only, no findings per author, no ranks, small groups hidden, and an audit log of who looked. [Privacy and data](privacy.md) covers the controls and lists every field Chakshu keeps about people.

## Insights

The [Insights](insights.md) page remains for a per-skill look at how findings fare over 30, 90 or 365 days, and every signed-in person can open it.
