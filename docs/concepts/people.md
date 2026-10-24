---
icon: material/account-group-outline
---

# People

The **People** view lists every member of the organization as a card, whether they use Chakshu or not. It answers rollout and budget questions: who has started asking for reviews, whose code gets reviewed automatically, and where the money goes. It isn't a measure of anyone's work, and it's built so it can't easily be used as one.

Only admins can open it, and only while names are turned on in the [privacy settings](privacy.md).

## Cards

Each card shows a person's avatar, name and login, whether they're an owner or a member, and an activity state for the period:

- **Asks for reviews:** they asked Chakshu for at least one review or reply.
- **Automatic reviews only:** Chakshu reviewed their pull requests on its own, but they didn't ask.
- **No activity:** neither.

Below that are the reviews they asked for, how many of their pull requests were reviewed, what the reviews they asked for cost, and when they were last active. People who have left the organization keep their history and are marked as not in the org.

Search by name or login, filter by role or activity, and sort by name, last activity, reviews requested, their PRs reviewed or cost. The page shows 12, 24 or 48 cards at a time. The period, repository, reviewer and trigger filters work as on the other analytics views.

The member list comes from GitHub every 15 minutes and whenever someone joins or leaves. **Refresh members** fetches it now. If GitHub can't be reached, the last list stays and the page says how old it is.

## A person's page

**View** on a card opens that person's page: tiles for the period against their own previous period, a daily chart, their usage by trigger, repository and reviewer, and a list of recent runs with whether each was requested, on their PR, or both.

Everyone can open their own page from **My activity**. It shows exactly what an admin sees about them. Admins can open anyone's page; other people can't.

## Export

**Export CSV** downloads the cards for the current selection. Admins can turn export off in the privacy settings. Every view and export of named people goes to the audit log.
