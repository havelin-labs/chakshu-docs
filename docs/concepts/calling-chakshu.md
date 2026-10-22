# Calling Chakshu

Nobody needs to remember reviewer names. People call Chakshu, and Chakshu works out which reviewers to run.

## Ways to ask

| Where | What to do |
|---|---|
| Pull request comment | `@acme/chakshu`, optionally with a note: `@acme/chakshu check the refund flow`. Chakshu reacts with 👀. |
| Reviewers list | Request `@acme/chakshu` like any reviewer. The request clears once the review is posted. |
| Portal | **Repositories**, then the repo, then **Review** next to the pull request, with an optional note. |
| Automatically | Reviewers with `auto` run on those PR events without anyone asking. |

The note travels with the request and shows up in the run's details.

## How Chakshu picks reviewers

1. Keep reviewers whose `repos` include this repository.
2. Of those, keep the ones whose `paths` match at least one changed file.
3. For automatic runs, keep the ones that list this PR event in `auto`.
4. For people asking, keep the ones owned by a team the person belongs to. Org owners can run any reviewer. In personal-account repos, everyone with write access can.
5. If nothing is left, run the `default` reviewer if it covers the repo. Otherwise reply with which reviewers cover it and who owns them.

## Who can ask

People with write, maintain or admin access to the repository. Anyone else gets a short reply, nothing runs and nothing is billed. Comments from bots are ignored.

## Asking twice

Asking again for the same commit doesn't start a second review while the first is queued, running or done. A new push, or a change to the reviewer, makes the next request a new review. After a push, that review looks only at what changed since the last one (see [How a review runs](how-a-review-runs.md#later-reviews-of-the-same-pr)). Findings already posted on the pull request aren't repeated, and Chakshu's own threads on code that has since been fixed are resolved.

## Spend cap

If an admin set a monthly spend cap and this month's reviews have reached it, Chakshu replies that reviews are paused and runs nothing new until the cap is raised or the month ends.

## Why `@acme/chakshu` and not `@chakshu`

A bare `@chakshu` would notify any GitHub user who happens to have that name, and a self-hosted app can't appear in GitHub's Reviewers list. An empty organization team called `chakshu` solves both: it autocompletes, shows up in the Reviewers list and notifies nobody.

Personal repos outside an organization can start a line with `/chakshu` instead.
