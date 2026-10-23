---
icon: material/connection
---

# Connectors

Connectors let a reviewer read from systems outside the code. The first one is Jira: a reviewer can open the issue a pull request is for and check the change does what the issue asks.

## Set one up

1. In **Settings → Connectors**, add a connector:
    - **Name**: what reviewers refer to, e.g. `jira`
    - **Kind**: Jira
    - **Base URL**: e.g. `https://acme.atlassian.net`
    - **Account email** and **API token**: an Atlassian account that can read the projects your teams use. Create the token at id.atlassian.com under Security.
2. Press **Test**. It should say who it's connected as.
3. In a reviewer's `reviewer.yaml`, add the connector's name:

    ```yaml
    connectors: [jira]
    ```

    The change takes effect when it merges in the reviewers repo.

## What the reviewer gets

For each review, Chakshu gives the agent one read-only tool, `get_issue`, which returns an issue's summary, type, status, priority, labels, parent and description. Issue keys in the pull request's title, branch name and description (like `PAY-412`) are listed in the prompt, so the agent knows which ones to read.

Use a skill to say what to check, for example:

```markdown
---
name: matches-ticket
description: The change does what the linked Jira issue asks, and nothing it doesn't.
---
- Read every linked issue with get_issue.
- Flag acceptance criteria the change doesn't cover.
- Flag behavior the change adds that the issue doesn't mention.
```

## Security

- The token is encrypted in the database and never shown again after you save it.
- Reviews run on GitHub Actions runners, but the tool doesn't: the runner forwards each call to Chakshu, which runs it with the stored token. The token never leaves the server, and the agent only sees the issue text.
- Chakshu only runs tools for connectors the reviewer lists.
- Issue text is treated like pull request text: marked as untrusted, so instructions inside it aren't followed.
- The tool can only read issues by key. It can't search, comment or change anything.
