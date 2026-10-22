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

If both are set, the API key is used, and the box shows which one reviews run on. Both are write-only. Using Bedrock or Vertex instead? Leave both empty and set the environment variables in [Configuration](../reference/configuration.md).

!!! warning "Subscription tokens"
    Anthropic's terms restrict using claude.ai subscriptions in third-party products. Check that your use is allowed before relying on a subscription token, and prefer an API key for shared or production use.

The same page sets the default model, the cost caps and the reviewers repo name.

## 6. Create the reviewers repo

Create `<org>/chakshu-reviewers` from the [template](https://github.com/havelin-labs/chakshu-reviewers-template) with **Use this template**, and make sure the app can see it. Then press **Sync from GitHub** on the **Reviewers** page. You should see the `default` reviewer marked ready.

## 7. Create the chakshu team

In your organization, create an empty team called `chakshu` and give it read access to the repos you want reviewed. It's the handle people mention and request; see [Calling Chakshu](../concepts/calling-chakshu.md).

## First review

Comment `@<org>/chakshu` on an open pull request. You'll see 👀 on your comment, a **Chakshu · default** check run, and a review a minute or two later. The **Reviews** page in the portal shows progress and cost.

## Permissions the app asks for

| Permission | Why |
|---|---|
| Contents: read | Fetch pull request code and the reviewers repo |
| Pull requests: write | Read diffs, post reviews, clear the chakshu review request |
| Checks: write | One check run per reviewer |
| Issues: write | React to mentions and reply when a request can't run |
| Organization members: read | Org roles and team membership |
| Metadata: read | Required by GitHub |

Events: `pull_request`, `issue_comment`, `push`. Installation events arrive without subscribing.

## Turn on webhooks

Needed for mentions, Reviewers-list requests and automatic reviews.

1. Give GitHub a public URL. On a server, that's `CHAKSHU_BASE_URL`. On a laptop, create a channel at [smee.io](https://smee.io), put it in `.env` as `CHAKSHU_WEBHOOK_URL`, and run `docker compose up -d` and `docker compose --profile smee up -d`.
2. If the app was created without webhooks, open its settings on GitHub (the owner's Settings, Developer settings, GitHub Apps, then the app). Tick **Webhook: Active**, set the URL from Chakshu's Settings page, leave the secret empty, and save.
3. Under **Permissions & events**, subscribe to **Pull request**, **Issue comment** and **Push**, and save. GitHub greys these out until the webhook is active.
4. In Chakshu, press **Settings → Webhooks → Connect webhooks**. Chakshu sets the webhook secret on GitHub itself.

To check, comment `@<org>/chakshu` on a pull request. You should see 👀 within a few seconds.
