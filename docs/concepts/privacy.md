---
icon: material/shield-account-outline
---

# Privacy and data

Chakshu keeps a few facts about people so admins can answer rollout and budget questions. This page lists every one of them, what each is for, where it shows and how long it's kept. You can attach it to a data protection impact assessment or a works agreement.

## Controls

Admins set these in the privacy card at the top of the [People](people.md) view.

| Setting | What it does | Default |
|---|---|---|
| Show people by name | Off hides the People view and other people's pages. Organization, repository, reviewer and team numbers stay. | On |
| Smallest group shown | Grouped numbers about fewer people than this are hidden, so a narrow filter can't single someone out. 1 turns it off; otherwise it's 3 to 100. | 5 |
| Keep people on reviews for (days) | Once a day, clears who asked for and who authored reviews older than this, and the people on older pull request milestones. Empty keeps them. 30 to 3650. | Empty |
| Allow CSV export of named people | Off refuses the People export. | On |

**Turn on works council mode** is one step for when a works council or employee agreement is pending: it turns names off, raises the smallest group to 10 and turns export off. Change any of them back once an agreement is in place.

Every change to these settings is recorded, with the old and new values.

## Audit log

Chakshu records each time someone opens the People view, opens another person's page or activity, exports people, or changes the privacy settings: who did it, when, which view and with what filters. Admins see the newest entries in the privacy card. Opening your own page isn't recorded.

## What Chakshu keeps about people

| Data | Where it's stored | Why | Where it shows | How long |
|---|---|---|---|---|
| Login, name and avatar of people who sign in to the portal | `users` | Sign-in and admin rights | Portal menu | Until the account is removed |
| Login, name, avatar, owner or member, and whether they're still in the org, for every org member | `org_members` | The People directory | People view | Kept after someone leaves, so their history keeps a name |
| Who asked for a review and who authored the pull request | `runs.requested_by`, `runs.pr_author` | Usage per person and per team | People, person pages, My activity, Teams | Cleared after the retention period, if one is set |
| Team memberships, with when each started and ended | `team_memberships` | Counting runs for the team people were in at the time | Teams view and filter | Ended memberships are deleted after the retention period |
| Who opened a pull request and who first reviewed it | `pr_events.actor` | Recorded with review speed; the view shows no names | Not shown | Cleared after the retention period |
| Who viewed or exported people analytics | `analytics_audit` | Accountability for looking at people | Privacy card | Kept |

Chakshu never stores findings per author, ranks people, or shows top or bottom lists. Review findings belong to the pull request and the reviewer, not to a person.

Pull request content (the diff, files and description) is read during a review in the repository's own GitHub Actions run and sent to Claude with your organization's credential. Chakshu's server stores the findings it posts, not the code.

The weekly Slack digest contains no names.
