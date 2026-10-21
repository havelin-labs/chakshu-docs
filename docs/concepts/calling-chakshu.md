# Calling Chakshu

!!! warning "Planned"
    Triggers are designed but not built yet.

Nobody needs to remember reviewer names. People call Chakshu, and Chakshu works out which reviewers to run.

## Ways to ask

| Where | What to do |
|---|---|
| Pull request comment | `@acme/chakshu`, optionally with a note: `@acme/chakshu check the refund flow` |
| Reviewers list | Request `@acme/chakshu` like any reviewer. The request clears once the review is posted. |
| Automatically | Reviewers with `auto` run when a PR touches their `paths`. |
| Portal | Open the PR, see which of your reviewers will run, click **Review**. |

## How Chakshu picks reviewers

1. Keep reviewers whose `repos` include this repository.
2. Of those, keep the ones whose `paths` match at least one changed file.
3. Of those, keep the ones owned by a team the person asking belongs to. Automatic runs skip this step.
4. Run each one that's left. If none are left, run the organization's `default` reviewer if there is one. Otherwise reply with which reviewers cover this repo and who owns them.

## Why `@acme/chakshu` and not `@chakshu`

A bare `@chakshu` would notify any GitHub user who happens to have that name. A self-hosted app also can't appear in GitHub's Reviewers list. An empty organization team called `chakshu` solves both: it autocompletes, shows up in the Reviewers list and notifies nobody.

Personal repos outside an organization can use `/chakshu` as a plain-text command instead.

## Who can ask

Only people with write access to the repository. Everyone else gets a short reply, no review runs and nothing is billed.
