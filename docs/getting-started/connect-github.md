# Connect GitHub

!!! warning "Planned"
    The setup flow below is designed but not built yet. It's next on the [roadmap](../project/roadmap.md).

Chakshu talks to GitHub as a GitHub App that belongs to your install. You won't create the app by hand or copy a private key around.

1. **Claim the instance.** On first start, `web` prints a one-time setup code to its logs (`docker compose logs web | grep "Setup code"`). Enter it in the portal. This stops anyone else who finds the URL from setting it up first.
2. **Create the app.** Pick your organization and click **Create on GitHub**. GitHub's manifest flow creates the app with the permissions and webhook Chakshu needs, then hands the credentials back. They're stored encrypted.
3. **Install it** on the organization, with access to the repos you want reviewed and to your reviewers repo.
4. **Sign in with GitHub.** Organization owners become Chakshu admins. You can add more with `CHAKSHU_ADMINS`.
5. **Add your Anthropic API key** in Settings.
6. **Create the `chakshu` team** in your organization: an empty team with read access to your repos. It's the handle people mention; see [Calling Chakshu](../concepts/calling-chakshu.md).

## Permissions the app asks for

| Permission | Why |
|---|---|
| Contents: read | Clone pull requests and read the reviewers repo |
| Pull requests: write | Read diffs and post reviews |
| Checks: write | One check run per reviewer |
| Issues: write | React to mentions and reply when a request can't run |
| Organization members: read | Check which teams the person asking belongs to |
| Metadata: read | Required by GitHub |

Events: `pull_request`, `issue_comment`, `push`.

## Running on a laptop

GitHub needs a public URL to deliver webhooks. Create a channel at smee.io, set `CHAKSHU_WEBHOOK_URL` to it, and forward it to `http://localhost:8000/webhooks/github`. Reviews started from the portal don't need webhooks.
