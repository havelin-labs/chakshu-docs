# Connect GitHub

Chakshu talks to GitHub as a GitHub App that belongs to your install. The portal creates it for you through GitHub's manifest flow, so you never copy a private key around.

## 1. Claim the instance

Open `CHAKSHU_BASE_URL`. You'll land on **Connect Chakshu to GitHub**. Enter the setup code from `docker compose logs web`. This proves you run the server, so nobody else who finds the URL can set it up first.

## 2. Create the app

Enter your GitHub organization, using its login from the URL (`github.com/acme`). You need to be an owner of it. Then choose an app name.

The app is private, and GitHub only lets a private app be installed on the account that owns it. So create it under the org whose repositories you want reviewed. Choose "personal account" only if you're reviewing your own repositories. App names are unique across all of GitHub, so the default is `chakshu-<org>`. Click **Create on GitHub**, check the details GitHub shows, and confirm.

GitHub sends you back to Chakshu with the app's keys. They're stored encrypted with `CHAKSHU_SECRET_KEY`.

!!! warning "Webhooks need a public URL"
    GitHub only delivers webhooks to URLs it can reach. If `CHAKSHU_BASE_URL` is `localhost` or a private address, the app is created without webhooks: the portal's Review button works, but mentions and automatic reviews don't. For a laptop, create a channel at [smee.io](https://smee.io), set `CHAKSHU_WEBHOOK_URL` to it, forward it to `http://localhost:8000/webhooks/github`, and do this step after restarting.

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

## 5. Add your API key

Go to **Settings** and paste your Anthropic API key. It's write-only: the page only shows that a key is set and its last characters. You can also set the default model, the cost cap per review and the reviewers repo name here.

Using Bedrock or Vertex instead? Leave the key empty and set the environment variables in [Configuration](../reference/configuration.md).

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
