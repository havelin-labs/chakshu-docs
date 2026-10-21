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

## Next

1. Publish these docs at chakshu.havelin.in/docs.
2. Tag `v0.1.0` after end-to-end testing.
3. Review only the commits added since the last review.
4. Resolve Chakshu's own comment threads when the code they point at changes.

## Later

- Jira and other context through MCP connectors (`connectors` in `reviewer.yaml`)
- Sandboxed scripts from skills, such as linters or schema checks
- Other engines (OpenAI, Copilot SDK)
- GitLab and Bitbucket
