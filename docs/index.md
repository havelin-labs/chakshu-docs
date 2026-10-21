# Chakshu

Chakshu (चक्षु, Sanskrit for "the eye") reviews pull requests on GitHub. You run it on your own server.

Every team in your organization keeps its own reviewer. A reviewer is a folder in a GitHub repo that holds the team's skills, rules and settings. When someone comments `@your-org/chakshu` on a pull request, Chakshu runs the reviewers that belong to that person's teams and match the files the PR changed. Each reviewer posts its own review and check. Reviews run on the Claude Agent SDK with read-only tools.

!!! note "Early days"
    Chakshu is under active development. The pages below say which parts work today and which are planned. The [roadmap](project/roadmap.md) shows what's next.

## What you need

- A server or laptop with Docker and Docker Compose
- Admin rights in the GitHub organization you want to review
- An Anthropic API key

## Where to start

- [Install with Docker Compose](getting-started/install.md) gets the app and its database running.
- [Reviewers](concepts/reviewers.md) explains how a team describes the review it wants.
- [Calling Chakshu](concepts/calling-chakshu.md) covers what people type on a pull request.
