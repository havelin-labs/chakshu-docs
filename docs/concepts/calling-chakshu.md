---
icon: material/at
---

# Calling Chakshu

Call Chakshu and it works out which reviewers fit the change. If you know the review you want, name the reviewer.

## Ways to ask

| Where | What to do |
|---|---|
| Pull request comment | `@acme/chakshu`, optionally with a note: `@acme/chakshu check the refund flow`. Chakshu reacts with 👀. |
| By name | `@acme/chakshu security` runs the `security` reviewer. Several work too: `@acme/chakshu security, payments: check the refund flow`. |
| Reviewers list | Request `acme/chakshu` like any reviewer to run every reviewer that covers the change, or request a reviewer's own team, like `acme/chakshu-security`, to run just that one. The request clears once the review is posted. |
| Portal | **Repositories**, then the repo. Toggle the reviewers you want, or none for the automatic choice, add an optional note and press **Review**. |
| Automatically | Reviewers with `auto` run on those PR events without anyone asking. |

The note travels with the request and shows up in the run's details.

## Naming reviewers

Words right after the mention that match a reviewer's name pick that reviewer. Case doesn't matter, and names can be separated by spaces, commas or a colon. Everything after the names is the note. A named reviewer runs even if its `repos` or `paths` don't cover the change, as long as it isn't broken.

In the portal, a name that doesn't match a working reviewer is refused with the list of available ones.

## One reviewer from the Reviewers list

GitHub's Reviewers list only shows people and teams, so each reviewer also gets its own team under `chakshu`, named `chakshu-<reviewer>`: `chakshu-security`, `chakshu-payments`, `chakshu-default`. They inherit the `chakshu` team's access to the repositories, so they show up in the list like any team. Requesting one is the same as naming that reviewer in a comment.

Chakshu creates these teams when you press **Create the chakshu teams** and keeps them in step with the reviewers repo on every sync: a new reviewer gets a team, and a removed one loses it. Broken reviewers don't get a team. Chakshu only ever removes teams it created itself, which say "Managed by Chakshu." in their description, so a team you add under `chakshu` by hand is left alone. The **Reviewers** page shows each reviewer's team.

## How Chakshu picks reviewers when you don't name one

1. Keep reviewers whose `repos` include this repository.
2. Of those, keep the ones whose `paths` match at least one changed file.
3. For automatic runs, keep the ones that list this PR event in `auto`.
4. If nothing is left, run the `default` reviewer if it covers the repo. Otherwise reply with the reviewers that exist and how to call one by name.

`default` is also an ordinary reviewer. The template gives it `repos: ["*"]` and no `paths`, so it matches every pull request and runs next to the others, not only when nothing else matches. Narrow its `repos` or `paths` if you only want it as the fallback.

Any team's reviewer can be run by anyone who may ask. `owner_team` says who looks after a reviewer, not who may use it.

## Who can ask

People with write, maintain or admin access to the repository. Anyone else gets a short reply, nothing runs and nothing is billed. Comments from bots are ignored.

## Asking twice

Asking again for the same commit doesn't start a second review while the first is queued, running or done. A new push, or a change to the reviewers repo, makes the next request a new review. After a push, that review looks only at what changed since the last one (see [How a review runs](how-a-review-runs.md#later-reviews-of-the-same-pr)). Findings already posted on the pull request aren't repeated, and Chakshu's own threads on code that has since been fixed are resolved.

## Spend cap

If an admin set a monthly spend cap and this month's reviews have reached it, Chakshu replies that reviews are paused and runs nothing new until the cap is raised or the month ends.

## Why `@acme/chakshu` and not `@chakshu`

A bare `@chakshu` would notify any GitHub user who happens to have that name, and a self-hosted app can't appear in GitHub's Reviewers list. An empty organization team called `chakshu` solves both: it autocompletes, shows up in the Reviewers list and notifies nobody. Its child teams, one per reviewer, do the same for single reviewers. Chakshu can create them for you during setup.

Personal repos outside an organization can start a line with `/chakshu` instead.
