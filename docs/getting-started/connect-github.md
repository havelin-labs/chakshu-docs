---
icon: material/github
---

# Connect GitHub

Chakshu talks to GitHub as a GitHub App that belongs to your install. The portal creates it for you through GitHub's manifest flow, so you never copy a private key around.

## 1. Claim the instance

Open `CHAKSHU_BASE_URL`. You'll land on **Connect Chakshu to GitHub**. Enter the setup code from `docker compose logs web`. This proves you run the server, so nobody else who finds the URL can set it up first.

## 2. Create the app

Enter your GitHub organization, using its login from the URL (`github.com/acme`). You need to be an owner of it. Then choose an app name.

The app is private, and GitHub only lets a private app be installed on the account that owns it. So create it under the org whose repositories you want reviewed. Choose "personal account" only if you're reviewing your own repositories. App names are unique across all of GitHub, so the default is `chakshu-<org>`. Click **Create on GitHub**, check the details GitHub shows, and confirm.

GitHub sends you back to Chakshu with the app's keys. They're stored encrypted with `CHAKSHU_SECRET_KEY`.

!!! warning "Webhooks need a public URL"
    GitHub only delivers webhooks to URLs it can reach. If `CHAKSHU_BASE_URL` is `localhost` or a private address, the app is created without webhooks: the portal's Review button works, but mentions and automatic reviews don't. You can turn them on later; see [Turn on webhooks](#turn-on-webhooks).

## 3. Install it

GitHub opens the install page next. Pick the repositories you want reviewed, plus the reviewers repo (step 6). You can change the selection later from **Change repository access** on the confirmation page.

## 4. Sign in

Click **Sign in with GitHub**. Who can sign in:

| Who | Signs in | Admin |
|---|---|---|
| Owner of the installed org | yes | yes |
| Member of the installed org | yes | no |
| Owner of an installed personal account | yes | yes |
| A login in `CHAKSHU_ADMINS` | yes | yes |
| Anyone else | no | |

## 5. Add Claude access

Go to **Settings → Claude access** and fill in one of:

- **Anthropic API key**, from console.anthropic.com, paid from Console credits.
- **Claude subscription token**: run `claude setup-token` with a Pro or Max account and paste the token. Reviews then count against that person's subscription limits.

If both are set, the API key is used, and the box shows which one reviews run on. Both are write-only. Reviews run in GitHub Actions, so saving a credential also copies it into the Actions secrets of every enabled repository.

!!! warning "Subscription tokens"
    Anthropic's terms restrict using claude.ai subscriptions in third-party products. Check that your use is allowed before relying on a subscription token, and prefer an API key for shared or production use.

The same page sets the default model, the cost caps and the reviewers repo name.

## 6. Create the reviewers repo

Create `<org>/chakshu-reviewers` from the [template](https://github.com/havelin-labs/chakshu-reviewers-template) with **Use this template**, and make sure the app can see it. Then press **Sync from GitHub** on the **Reviewers** page. You should see the `default` reviewer marked ready.

## 7. Create the chakshu team

Press **Create the chakshu team**, either on the page GitHub sends you back to after installing or under **Settings → Review team**. It creates an empty, closed team called `chakshu` with notifications off and read access to the installed repositories. That's the handle people mention and request; see [Calling Chakshu](../concepts/calling-chakshu.md). If the team already exists, Chakshu leaves it alone. You can also create it by hand.

## 8. Set up GitHub Actions

Reviews run in each repository's Actions. On each repository's page in the portal, press **Set up GitHub Actions** and merge the pull request it opens. GitHub's runners also need to reach Chakshu. See [Set up GitHub Actions](github-actions.md).

## First review

Comment `@<org>/chakshu` on an open pull request. You'll see 👀 on your comment, a **Chakshu · default** check run whose Details link opens the review's Actions run, and a review a minute or two later. The **Reviews** page in the portal shows progress and cost.

## Permissions the app asks for

| Permission | Why |
|---|---|
| Contents: read and write | Fetch code and the reviewers repo; open pull requests with the workflow and with reviewer changes. Chakshu never pushes to a default branch. |
| Pull requests: write | Read diffs, post reviews and replies, resolve its own threads, clear the chakshu review request |
| Checks: write | One check run per reviewer |
| Issues: write | React to mentions and reply when a request can't run |
| Actions: read and write | Start the review workflow and hear when it ends |
| Workflows: read and write | Propose `.github/workflows/chakshu.yml` |
| Secrets: read and write | Keep the Claude credential in each repository's Actions secrets |
| Organization members: read and write | Org roles, and creating the empty `chakshu` team |
| Metadata: read | Required by GitHub |

Events: `pull_request`, `issue_comment`, `pull_request_review_comment`, `push`, `workflow_run`. Installation events arrive without subscribing.

An app created before a feature needed a permission won't have it. **Settings → GitHub Actions** lists what the app and each installation are missing, with links to the app's permission page and to the page where an owner accepts the change.
## Turn on webhooks

Needed for mentions, Reviewers-list requests and automatic reviews.

1. Give GitHub a public URL. On a server, that's `CHAKSHU_BASE_URL`. On a laptop, create a channel at [smee.io](https://smee.io), put it in `.env` as `CHAKSHU_WEBHOOK_URL`, and run `docker compose up -d` and `docker compose --profile smee up -d`.
2. If the app was created without webhooks, open its settings on GitHub (the owner's Settings, Developer settings, GitHub Apps, then the app). Tick **Webhook: Active**, set the URL from Chakshu's Settings page, leave the secret empty, and save.
3. Under **Permissions & events**, subscribe to **Pull request**, **Issue comment**, **Pull request review comment**, **Push** and **Workflow run**, and save. GitHub greys these out until the webhook is active.
4. In Chakshu, press **Settings → Webhooks → Connect webhooks**. Chakshu sets the webhook secret on GitHub itself.

To check, comment `@<org>/chakshu` on a pull request. You should see 👀 within a few seconds.
