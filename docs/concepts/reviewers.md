# Reviewers

A reviewer is the review one team wants for its product. It lives as a folder in a single repo in your organization, `chakshu-reviewers` by default. GitHub already knows who is on which team, so it also decides who can edit a reviewer and who can call it.

Start from the [template repo](https://github.com/havelin-labs/chakshu-reviewers-template).

```text
acme/chakshu-reviewers
├─ CODEOWNERS                   # each team owns its folder
├─ shared/skills/               # skills any reviewer can include
│  └─ secure-coding/SKILL.md
└─ reviewers/
   ├─ default/                  # the org-wide fallback
   └─ payments/                 # owned by @acme/payments
      ├─ reviewer.yaml
      ├─ RULES.md               # team conventions
      └─ skills/
         ├─ pci-dss/
         │  ├─ SKILL.md
         │  └─ checklist.md     # supporting files the skill can read
         └─ money-handling/
            ├─ SKILL.md
            └─ examples/rounding.md
```

## reviewer.yaml

```yaml
name: payments-reviewer
product: Payments
description: Money, refunds, PCI rules
owner_team: payments             # GitHub team slug; its members can call this reviewer
model: sonnet
repos: [payments-api, billing-*]
paths: ["src/payments/**", "src/refunds/**"]
auto: [opened, ready_for_review] # run on its own for these PR events
skills:
  - pci-dss                      # ./skills/pci-dss
  - money-handling
  - shared/secure-coding         # from shared/skills
rules: RULES.md
limits:
  max_usd: 2
  max_findings: 10
  min_severity: medium
fail_check_on: high
```

| Field | Default | Meaning |
|---|---|---|
| `name` | required | Lowercase letters, digits and dashes. Unique in the repo. |
| `owner_team` | required | GitHub team slug. Only its members (and org owners) can call this reviewer. |
| `product`, `description` | none | Shown in the portal. |
| `model` | portal default | Model alias (`sonnet`, `opus`, `haiku`) or a full model name. |
| `repos` | `["*"]` | Repo names or glob patterns this reviewer covers. |
| `paths` | every file | Run only when the PR changes a matching file. Gitignore-style patterns. |
| `auto` | `[]` | PR events that start this reviewer without anyone asking: `opened`, `reopened`, `ready_for_review`, `synchronize`. Drafts are skipped. |
| `skills` | `[]` | Folders under `skills/`, or `shared/<name>`. |
| `rules` | `RULES.md` | A file in the folder with the team's conventions. Optional. |
| `limits.max_usd` | portal default | Hard cost cap for one review. |
| `limits.max_findings` | 10 | The most findings posted per review, most severe first. |
| `limits.min_severity` | `medium` | Findings below this aren't posted. |
| `fail_check_on` | never | Lowest severity that turns the check red. |
| `connectors` | `[]` | Names of [connectors](connectors.md) this reviewer can read from, e.g. `[jira]`. |
| `tools` | `[]` | Accepted for forward compatibility. Reviews always run read-only tools. |

Unknown fields are errors, so a typo shows up instead of silently doing nothing.

## The default reviewer

A reviewer named `default` is the fallback. When someone asks for a review and none of their teams' reviewers fit, Chakshu runs `default` if it covers the repo. Without one, Chakshu replies with which reviewers do cover the repo and who owns them.

## Skills

A skill is a folder with a `SKILL.md`, the same format the Claude Agent SDK uses:

```markdown
---
name: money-handling
description: Amounts are integer minor units, rounding is explicit, currencies never mix.
---
- Amounts must be integer cents. Flag floats or decimals without a rounding mode.
- Refunds can never exceed the captured amount. See examples/rounding.md.
```

Anything else in the folder (checklists, examples) is reference material the reviewer can read while it works. Short and specific skills beat long general ones.

## Changing a reviewer

Edit the folder in a pull request to the reviewers repo, like any other code. When it merges to the default branch, Chakshu resyncs automatically through the `push` webhook. Admins can also press **Sync from GitHub** on the **Reviewers** page.

Each review records the reviewer commit it used, so old results stay explainable after you edit it. A reviewer with problems is listed as **broken** with the exact errors, and other reviewers keep working.
