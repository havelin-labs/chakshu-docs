# Reviewers

!!! warning "Planned"
    Reviewer folders and their sync are the next big piece of work. This page describes the format so teams can start writing them.

A reviewer is the review one team wants for its product. It lives as a folder in a single repo in your organization, `chakshu-reviewers` by default. GitHub already knows who is on which team, so it also decides who can edit a reviewer and who can call it.

```text
acme/chakshu-reviewers
├─ CODEOWNERS                   # each team owns its folder
├─ shared/skills/               # skills any reviewer can include
│  └─ secure-coding/SKILL.md
└─ reviewers/
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
owner_team: payments            # GitHub team slug; members can call and edit it
model: sonnet
repos: [payments-api, billing-*]
paths: ["src/payments/**", "src/refunds/**"]
auto: [opened, ready_for_review] # run on its own when a PR touches paths
skills:
  - pci-dss                      # ./skills/pci-dss
  - money-handling
  - shared/secure-coding         # from shared/skills
rules: RULES.md
limits: { max_usd: 2, max_findings: 10, min_severity: medium }
fail_check_on: high
```

| Field | Meaning |
|---|---|
| `owner_team` | The team that owns this reviewer. Only its members can call it. |
| `repos` | Repo names or glob patterns this reviewer covers. |
| `paths` | Only run when the PR changes a matching file. Leave it out to match every file. |
| `auto` | PR events that start this reviewer without anyone asking. |
| `skills` | Folders under `skills/`, or `shared/<name>`. |
| `limits.max_usd` | Hard cost cap for one review. |
| `fail_check_on` | Lowest severity that turns the check red. Leave it out to never fail. |

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

Anything else in the folder (checklists, examples) is reference material the reviewer can read while it works.

## Changing a reviewer

Edit the folder in a pull request to the reviewers repo, like any other code. Chakshu picks up the change when it lands on the default branch. Each review records the commit of the reviewer it used, so old results stay explainable after you edit it.
