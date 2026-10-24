---
icon: material/account-multiple-outline
---

# Teams

The **Teams** view shows reviews, spend and outcomes for each GitHub team in the organization, and the **Team** filter limits every analytics view to one team.

## How runs are matched to teams

A run counts for a team if its pull request's author, or the person who asked for the review, was a direct member of the team when the run happened. So:

- A run can count for several teams. Team rows overlap and don't add up to the total.
- Members of a child team aren't counted in its parent.
- When someone moves team, their earlier runs stay with the old team. A reorg doesn't rewrite history.

Chakshu learns teams from GitHub nightly, from the `team` and `membership` webhooks as changes happen, and when an admin presses **Refresh teams**. Memberships it finds on its first sync have no start date, because they began before Chakshu was watching, so runs from before that sync count for the teams people were in at the time of the sync. Deleted teams keep their history and are marked as deleted in the filter.

## Small teams

The [smallest group size](privacy.md) applies to the final slice. The Teams view hides any team where fewer people than that appear in the selected period and filters, and says how many it hid. Filtering by such a team shows a message instead of numbers.

## Setup

The app's members permission covers this. Existing apps need the **Team** and **Membership** events turned on under the app's Permissions and events page; until then teams only change on the nightly sync and with Refresh teams.
