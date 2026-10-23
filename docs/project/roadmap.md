---
icon: material/map-outline
---

# Roadmap

Work lands in small pull requests, one concern each, tracked in [havelin-labs/chakshu](https://github.com/havelin-labs/chakshu/issues).

## Done (0.1)

- Scaffold, Postgres schema, deterministic IDs, encrypted settings
- React portal in the Havelin design system
- GitHub App setup through the manifest flow
- Sign in with GitHub, org roles and admins
- Reviewer catalog synced from each org's reviewers repo, plus a template repo
- Webhooks: mentions, review requests, automatic runs, reviewers-repo sync
- Queue and worker on Postgres
- Review engine on the Claude Agent SDK with read-only tools
- Publisher: inline comments, suggestions, no repeats, one check run per reviewer
- Portal pages for reviews, repositories and settings
- Release workflow with signed multi-arch images

## Done (0.2)

- Webhooks stored in Postgres and handled by the worker, with retries and a failures list
- Graceful worker shutdown and heartbeat-based recovery
- Monthly spend cap
- Later reviews look only at commits added since the last one
- Chakshu resolves its own outdated threads
- Insights: per-skill fixed and dismissed rates
- Jira connector

## Done (0.3)

- Reviews run in GitHub Actions in the reviewed repository, with logs in its Actions tab
- Repository setup from the portal: workflow pull request and Actions secrets
- Missing app permissions listed in Settings
- Call any reviewer by name
- Create and edit reviewers in the portal, through pull requests
- Chakshu answers replies to its comments
- The chakshu team is created during setup
- A Cloudflare tunnel profile for laptops
- Analytics for owners (usage, outcomes and spend by repository, reviewer, skill and person) and My activity for everyone

## Next

1. Tag a release, so the workflow can pin a version of the action.
2. Deploy to a server and publish these docs at chakshu.havelin.in/docs.

## Planned

- A People directory: a card for every member and a page per person ([#89](https://github.com/havelin-labs/chakshu/issues/89))
- Analytics: teams, spend alerts and a weekly digest, token counts, review speed, and retention controls ([#84](https://github.com/havelin-labs/chakshu/issues/84) to [#88](https://github.com/havelin-labs/chakshu/issues/88))

## Later

- More connectors: Linear, Confluence, Sentry
- Sandboxed scripts from skills, such as linters or schema checks
- Other engines (OpenAI, Copilot SDK)
- GitLab and Bitbucket
