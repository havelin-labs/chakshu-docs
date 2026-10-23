---
icon: material/play-circle-outline
---

# Set up GitHub Actions

Reviews run in GitHub Actions, in the pull request's own repository. Each review is a short workflow run, so anyone with access to the repo can open the **Actions** tab and watch it: which files the reviewer read, which tools it called, and what it found. Chakshu's server only coordinates. It never runs the model itself.

Each repository needs three things:

1. the workflow file `.github/workflows/chakshu.yml` on its default branch
2. the Claude credential in its Actions secrets
3. a way for GitHub's runners to reach Chakshu

## 1. Add the workflow

Open the repository in the portal (**Repositories**, then the repo). The **GitHub Actions** card says whether it's ready. Admins see **Set up GitHub Actions**, which:

- opens a pull request that adds `.github/workflows/chakshu.yml`, and
- writes the Claude credential into the repository's secrets.

Merge the pull request and the card turns **ready**. Chakshu never pushes to a default branch, so branch protection and code owners still apply. If a later Chakshu version changes the workflow, the card says it's outdated and the same button proposes the update.

The workflow only runs when Chakshu starts it (`workflow_dispatch`). It never runs on pushes or pull requests by itself. It has two permissions: `contents: read` to check out the pull request, and `id-token: write` to prove to Chakshu which run it is.

## 2. Claude access in secrets

Chakshu keeps two repository secrets, `CHAKSHU_ANTHROPIC_API_KEY` and `CHAKSHU_CLAUDE_CODE_OAUTH_TOKEN`, in step with **Settings → Claude access**. Saving a new key or token there copies it to every enabled repository straight away. **Settings → GitHub Actions → Copy the credential to all repositories** does the same on demand.

These are repository secrets rather than organization secrets because GitHub Free doesn't share organization secrets with private repositories.

!!! warning "Who can read the secret"
    Anyone with write access to a repository can write a workflow that prints its secrets. Treat the Claude credential as shared with everyone who can push to the repositories Chakshu reviews.

## 3. Let runners reach Chakshu

A runner fetches its job from Chakshu and sends the findings back, so it needs a URL it can reach from the internet. That's `CHAKSHU_RUNNER_URL`, or `CHAKSHU_BASE_URL` when it isn't set. **Settings → GitHub Actions** shows a warning if the URL isn't public.

On a server with a public `https://` address there's nothing to do. On a laptop, use the bundled Cloudflare tunnel:

```bash
docker compose --profile tunnel up -d
docker compose logs tunnel | grep trycloudflare.com
# put that https://<random>.trycloudflare.com address in .env as CHAKSHU_RUNNER_URL, then
docker compose up -d web worker
```

A quick tunnel gets a new address every time it starts, and can drop after a few hours. Repeat the steps when that happens, or use a named Cloudflare tunnel or a real server for anything longer-lived.

## The action

The workflow runs the `havelin-labs/chakshu` action. While that repository is private, allow repositories in your organization to use it: in its **Settings → Actions → General → Access**, choose access from repositories in the organization. Admins can point the workflow at another copy of the action with the `actions_action` setting (`owner/repo@ref`).

## App permissions

Running reviews in Actions needs these on top of the basics: **Actions**, **Contents**, **Secrets** and **Workflows** set to read and write, and the **Workflow run** event. Apps created by Chakshu ask for them from the start. For an older app, **Settings → GitHub Actions** lists exactly what the app and each installation are missing, with links to the pages where you add and accept them. See [Connect GitHub](connect-github.md#permissions-the-app-asks-for) for the full list.

## Where to look when something's off

- The check on the pull request (**Chakshu · <reviewer>**) links to the Actions run.
- The run page in the portal shows the status, the Actions run link and any error.
- If the Actions run ends without sending a result, for example because it was cancelled or couldn't reach Chakshu, the review fails with that reason. Chakshu also gives up on runs that haven't reported back after 45 minutes.
