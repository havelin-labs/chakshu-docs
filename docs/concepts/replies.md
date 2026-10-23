---
icon: material/message-reply-text-outline
---

# Replies

People can talk back to Chakshu. Reply to one of its inline comments, and the reviewer that posted the finding reads the thread and answers there.

```text
Chakshu:  Medium: some wrong-length codes never reach the new 400 check
You:      Why would a wrong-length code get a 422 before this check?
Chakshu:  FastAPI validates ClaimRequest before the handler runs. code is still
          Field(min_length=4, max_length=20), so "ABC" fails validation and gets
          a 422. Loosen the model if the ticket wants 400 for every length.
```

## What happens

1. GitHub sends a `pull_request_review_comment` event for your reply. Chakshu reacts with 👀.
2. It checks the thread is really one of its findings: the first comment must come from Chakshu's app and carry a finding marker.
3. A short reply job runs in GitHub Actions, like a review, with the thread, the code at the pull request's latest commit and the reviewer's skills. It's capped at 15 turns and $0.50, and it gets no connector tools.
4. The answer is posted in the thread. The reviewer also decides an outcome:
    - **open:** the finding still stands.
    - **fixed:** the code at the latest commit no longer has the problem. Chakshu checks the code rather than taking "fixed in abc123" on trust.
    - **dismissed:** the finding was wrong, or the behaviour is intended for a reason that holds up.

    For fixed and dismissed, Chakshu resolves the thread and records the outcome in [Insights](insights.md).

## Who gets an answer

The pull request's author and anyone with write access to the repository. Bots, including Chakshu itself, never do. Each thread gets at most five answers, and nothing runs once the monthly spend cap is reached. Chakshu says so in the thread instead.

## Setup

The app needs the **Pull request review comment** event. Apps created by Chakshu subscribe to it. For an older app, **Settings → GitHub Actions** says it's missing.
