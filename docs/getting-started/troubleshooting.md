# Troubleshooting

## My organization's repositories don't show up

Check who owns the app: open its settings on GitHub, under Settings, Developer settings, GitHub Apps. If it lives under your personal account, it can only be installed there. Fix it without reinstalling Chakshu:

1. In the app's settings, open **Advanced**, choose **Transfer ownership** and enter the organization. An org owner approves it.
2. Install the app on the organization (`https://github.com/apps/<app-name>/installations/new`) and pick the repositories.
3. In Chakshu, press **Sync repositories** on the Repositories page.

The app ID and keys don't change, so nothing in Chakshu needs reconfiguring.

## There's no setup code in the logs

The code only appears while no GitHub App is connected. If you've already connected one, sign in instead. If `web` can't reach the database at startup it logs a warning rather than a code. Check `curl localhost:8000/readyz` and `docker compose logs db`.

## "That GitHub account isn't a member of an organization this Chakshu serves"

You signed in with an account that isn't in any installed org. Sign in with an org member, or add the login to `CHAKSHU_ADMINS` and restart `web`.

## Mentions do nothing

Work through these in order:

1. **Webhooks configured?** In GitHub, open the app's settings, then **Advanced**, to see recent deliveries. No deliveries means the app has no webhook URL (see the warning in [Connect GitHub](connect-github.md)).
2. **Deliveries failing?** A 401 means the webhook secret doesn't match. A 503 means Chakshu has no webhook secret stored, which happens when the app was created without a public URL. Recreating the app with a reachable URL fixes both.
3. **Right handle?** It's `@<org>/chakshu`, and the `chakshu` team has to exist. Quoted text and code blocks are ignored.
4. **Repo turned off?** Check **Repositories** in the portal.

## Chakshu replied "only people with write access…"

The person asking needs write, maintain or admin access to the repository.

## Chakshu replied that no reviewer covers this change

No reviewer's `repos` and `paths` match, and there's no `default` reviewer for the repo. The reply lists the reviewers that exist. Call one by name (`@<org>/chakshu security`) or add a reviewer that covers the repo.

## The check says "Review didn't finish"

Open the run in the portal for the full error. Common causes:

- **No Claude credential in the repository's secrets**: set one in **Settings**, which copies it to every enabled repository, or press **Copy the credential to all repositories**.
- **Cost cap reached** (`error_max_budget_usd`): raise `limits.max_usd` in the reviewer or the default in **Settings**, or narrow the reviewer's `paths`.
- **Turn limit** (`error_max_turns`): the change is very large. Split the pull request or narrow `paths`.

GitHub and network errors are retried automatically up to three times before a run fails.

## Chakshu replied that reviews are paused

This month's spend reached the monthly cap in **Settings**. An admin can raise or clear it; new requests work straight away.

## A webhook shows up under "Failed webhooks"

The **Status** page lists webhooks that failed three times, with the error. Fix the cause (often the app lost access to a repo), then an admin can press **Retry**.

## The check says the repository doesn't have the Chakshu workflow

The workflow isn't on the default branch yet. Open the repository in the portal, press **Set up GitHub Actions** and merge the pull request. See [Set up GitHub Actions](github-actions.md).

## The Actions run fails with "can't reach Chakshu"

The runner couldn't reach `CHAKSHU_RUNNER_URL`. On a laptop, the quick tunnel probably restarted with a new address: get the new one from `docker compose logs tunnel`, update `.env` and run `docker compose up -d web worker`. **Settings → GitHub Actions** warns when the URL isn't public at all.

## The Actions run can't find the action

`havelin-labs/chakshu` is private and your repositories aren't allowed to use it. In that repository's **Settings → Actions → General → Access**, allow repositories in the organization.

## "The app isn't allowed to start workflows here"

The app is missing the Actions permission, or an owner hasn't accepted it. **Settings → GitHub Actions** lists what's missing, with links.

## The check says "Superseded by a newer commit"

Someone pushed before the review started, so the queued run was for an old commit. Ask again; the new review covers the latest commit.

## A reviewer shows "broken"

The **Reviewers** page lists the exact problems: a typo in `reviewer.yaml`, a skill without a `SKILL.md`, or a name used twice. Fix it in the reviewers repo. The page updates when the fix merges (or when you press **Sync from GitHub**).
