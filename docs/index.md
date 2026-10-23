# Chakshu

Chakshu (चक्षु, Sanskrit for "the eye") reviews pull requests on GitHub. You run it on your own server.

Every team in your organization keeps its own reviewer: a folder in a GitHub repo with the team's skills, rules and settings. When someone comments `@your-org/chakshu` on a pull request, Chakshu runs the reviewers that match the files the PR changed, or the ones named in the comment. Each reviewer posts its own review and check run. Reviews run in GitHub Actions in the pull request's own repository, so the logs are in its Actions tab, on the Claude Agent SDK, which can only read the code.

!!! note "Version 0.1"
    Everything on these pages works in the current code. It hasn't had a tagged release yet, so expect rough edges and tell us about them in [GitHub issues](https://github.com/havelin-labs/chakshu/issues).

## What you need

- A server or laptop with Docker and Docker Compose
- Owner rights in the GitHub organization you want to review
- Claude access: an Anthropic API key or a Claude Pro or Max subscription token
- GitHub Actions in the repositories you want reviewed

## Where to start

1. [Install with Docker Compose](getting-started/install.md) gets the app and its database running.
2. [Connect GitHub](getting-started/connect-github.md) creates the GitHub App.
3. [Set up GitHub Actions](getting-started/github-actions.md) gets each repository ready for reviews.
4. [Reviewers](concepts/reviewers.md) explains how a team describes the review it wants.
5. [Calling Chakshu](concepts/calling-chakshu.md) covers what people do on a pull request.

Something not working? See [Troubleshooting](getting-started/troubleshooting.md).
