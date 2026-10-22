# Troubleshooting

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

## Chakshu replied that no reviewer belongs to your teams

None of the reviewers that cover this repo and these files are owned by a team you're on, and there's no `default` reviewer. Ask someone on one of the listed teams, or add a reviewer for your team.

## The check says "Review didn't finish"

Open the run in the portal for the full error. Common causes:

- **No API key**: set one in **Settings**.
- **Cost cap reached** (`error_max_budget_usd`): raise `limits.max_usd` in the reviewer or the default in **Settings**, or narrow the reviewer's `paths`.
- **Turn limit** (`error_max_turns`): the change is very large. Split the pull request or narrow `paths`.

GitHub and network errors are retried automatically up to three times before a run fails.

## Chakshu replied that reviews are paused

This month's spend reached the monthly cap in **Settings**. An admin can raise or clear it; new requests work straight away.

## A webhook shows up under "Failed webhooks"

The **Status** page lists webhooks that failed three times, with the error. Fix the cause (often the app lost access to a repo), then an admin can press **Retry**.

## The check says "Superseded by a newer commit"

Someone pushed before the review started, so the queued run was for an old commit. Ask again; the new review covers the latest commit.

## A reviewer shows "broken"

The **Reviewers** page lists the exact problems: a typo in `reviewer.yaml`, a skill without a `SKILL.md`, or a name used twice. Fix it in the reviewers repo. The page updates when the fix merges (or when you press **Sync from GitHub**).
