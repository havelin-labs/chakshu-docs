---
icon: material/timer-outline
---

# Review speed

The **Review speed** view shows how long pull requests wait, per repository, for the PRs Chakshu reviewed and the ones it didn't:

- **First review:** hours from ready for review (or opened, if it never was a draft) to the first review by a person.
- **Merge:** hours from the same start to merge.

Each shows the median and the P90, the time nine in ten PRs beat. PRs are included if they were opened in the selected period. Only the period and repository filters apply.

## What counts

- A review counts if it approves, requests changes or comments, and comes from someone who isn't the PR's author and isn't a bot. Chakshu's own reviews don't count.
- PRs opened by bots, such as Dependabot, are left out.
- A PR counts as reviewed by Chakshu if a Chakshu review on it finished.
- Every repository in the installation is recorded, including ones Chakshu doesn't review, so there's always something to compare with.
- Values from fewer than 5 PRs are greyed out. That's too few to read much into.

## Reading it

This compares two groups of pull requests. It doesn't show that Chakshu made reviews faster or slower. Teams often ask for a Chakshu review on some PRs and not others, and those PRs can differ in size, urgency and author. Compare a repository with its own numbers over time rather than with other repositories.

## Setup

Chakshu records PR milestones from the `pull_request` and `pull_request_review` webhooks. Existing apps need the **Pull request review** event turned on under the app's Permissions and events page. Times start when the event is on; earlier PRs have no review times.
