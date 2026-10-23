---
icon: material/home-outline
hide:
  - navigation
---

# Chakshu

Chakshu (चक्षु, Sanskrit for "the eye") reviews pull requests on GitHub. You run it on your own server, each team brings its own reviewer, and every review runs in the pull request's own GitHub Actions.

<div class="grid cards" markdown>

-   :material-account-group-outline:{ .lg } **A reviewer per team**

    Each team keeps a folder of skills and rules in one repo. Calling `@your-org/chakshu` runs the ones that fit the change, or the ones you name.

-   :material-play-circle-outline:{ .lg } **Runs in GitHub Actions**

    Reviews run in the reviewed repository, so the logs are in its Actions tab and your server stays small.

-   :material-shield-lock-outline:{ .lg } **Read-only by design**

    The reviewer can read code and nothing else. Connector tokens such as Jira stay on your server.

-   :material-chart-box-outline:{ .lg } **See what it costs**

    Usage, outcomes and spend by repository, reviewer and person, with a forecast against your monthly cap.

</div>

## Get started

<div class="grid cards" markdown>

-   :material-docker:{ .lg } **[Install](getting-started/install.md)**

    Run Chakshu and Postgres with Docker Compose.

-   :material-github:{ .lg } **[Connect GitHub](getting-started/connect-github.md)**

    Create the GitHub App in a few clicks and add Claude access.

-   :material-play-circle-outline:{ .lg } **[Set up GitHub Actions](getting-started/github-actions.md)**

    One button per repository adds the workflow and its secrets.

-   :material-at:{ .lg } **[Call Chakshu](concepts/calling-chakshu.md)**

    Mention `@your-org/chakshu` on a pull request, or press Review in the portal.

</div>

## What you need

- A server or laptop with Docker and Docker Compose
- Owner rights in the GitHub organization you want reviewed
- Claude access: an Anthropic API key or a Claude Pro or Max subscription token
- GitHub Actions in the repositories you want reviewed

!!! note "Before the first release"
    Everything here works on `main` and has been run against a real GitHub organization. There's no tagged release yet, so tell us about rough edges in [GitHub issues](https://github.com/havelin-labs/chakshu/issues).

Something not working? See [Troubleshooting](getting-started/troubleshooting.md).
