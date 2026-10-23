---
icon: material/chart-box-outline
---

# Analytics

**Analytics** shows admins how Chakshu is used across the organization and what it costs. **My activity** shows each person their own usage.

Every view takes a period (7, 30 or 90 days, or a year) and can be filtered by repository, reviewer and trigger. The filters live in the page address, so you can share a view by sharing its link. Times are UTC.

## Views

| View | What it shows |
|---|---|
| Overview | Reviews, pull requests reviewed, spend, cost per fixed finding, findings posted and fixed, fix rate and the share of reviews someone asked for, each against the previous period of the same length. Daily reviews and daily spend. |
| Repositories, Reviewers, Skills | For each: reviews, PRs, spend, findings posted, fixed, dismissed and open, fix rate and cost per fixed finding. Export to CSV. Pick a reviewer to filter everything by it, or a skill to see its findings. |
| People | A usage ledger: reviews each person asked for, how many of their pull requests were reviewed, how many of those ran automatically, and what the reviews they asked for cost. |
| Cost | This month's spend against the monthly cap, a straight-line projection, the day the cap would be reached at the current rate, reviews stopped by the per-review cap, and spend by repository, reviewer, model and trigger. |
| Findings | The newest 200 findings with their outcome and how it was decided. |

## Definitions

- **Review:** a finished run that wasn't an answer to a [reply](replies.md).
- **Spend:** the cost of every run in the period, failed ones included, because they were paid for.
- **Fix rate:** fixed ÷ (fixed + dismissed). Findings still open have no verdict and are left out.
- **Fixed / dismissed:** a finding is fixed when the code under Chakshu's comment changes, and dismissed when someone resolves the thread without changing the code. Either can also be decided when the reviewer answers a reply. The Findings view says which happened.
- **Asked for:** reviews someone requested (a mention, a review request, the portal or a reply), as opposed to automatic reviews.
- **Cost per fixed finding:** spend ÷ findings fixed in the period.

## People and privacy

Per-person numbers help with rollout and budget questions: who uses Chakshu, whose code it reviews, and which requests the money goes to. They are a poor measure of anyone's work and easy to misread, so Chakshu limits them:

- The People view lists usage only. There are no findings per author, no ranks and no top or bottom lists, and it's sorted by login.
- If a selection covers fewer people than the smallest group size (5 by default, never below 3), it's hidden, so a narrow filter can't single someone out.
- Admins can turn names off entirely, for example while a works council or employee agreement is pending.
- Every view and export of named people is recorded in an audit log.
- Everyone can open **My activity** and see exactly the row admins see about them.

Admins set the names switch and the smallest group size at the top of the People view.

## Insights

The [Insights](insights.md) page remains for a per-skill look at how findings fare over 30, 90 or 365 days, and every signed-in person can open it.
