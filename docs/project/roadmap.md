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

## Next

1. Tag a release after end-to-end testing.
2. Publish these docs at chakshu.havelin.in/docs.
3. Per-skill stats: which skills' findings get fixed and which get dismissed.

## Later

- Jira and other context through MCP connectors (`connectors` in `reviewer.yaml`)
- Sandboxed scripts from skills, such as linters or schema checks
- Other engines (OpenAI, Copilot SDK)
- GitLab and Bitbucket
