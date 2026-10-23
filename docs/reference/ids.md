---
icon: material/identifier
---

# Deterministic IDs

Every row in Chakshu's database has a UUIDv5 primary key. The same inputs always give the same ID, which is what makes retries and resyncs safe.

```text
uuid5(DNS, "chakshu.havelin.in")                      -> PRODUCT_NS
  = d529e7b7-dbda-568d-af0a-d9f007824bf1 (never changes)
uuid5(PRODUCT_NS, "v1|instance|<github host>|app|<id>") -> one namespace per install
uuid5(install namespace, "v1|<kind>|<part>|...")         -> row id
```

| Entity | Hashed name |
|---|---|
| org, repo, user, team | <code>v1&#124;org&#124;&lt;github id&gt;</code> |
| reviewer | <code>v1&#124;reviewer&#124;&lt;org github id&gt;&#124;&lt;name&gt;</code> |
| pull request | <code>v1&#124;pr&#124;&lt;repo github id&gt;&#124;&lt;number&gt;</code> |
| work key | <code>v1&#124;work&#124;&lt;pr id&gt;&#124;&lt;head sha&gt;&#124;&lt;reviewer sha&gt;&#124;&lt;reviewer id&gt;</code> |
| finding | <code>v1&#124;finding&#124;&lt;pr id&gt;&#124;&lt;reviewer id&gt;&#124;&lt;path&gt;&#124;&lt;category&gt;&#124;&lt;title&gt;</code> |

## Why it helps

- A webhook GitHub delivers twice maps to the same row, so the second one is dropped.
- Two requests for the same reviewer, PR and commit share one review.
- Each posted comment carries its finding ID in a hidden marker, so a re-run doesn't repeat it.
- Two installs never share a namespace (different GitHub App IDs), so exports never collide.

## Rules

- Hash GitHub's numeric IDs, not logins or repo names. Names can be renamed.
- Parts are escaped, so `("a|b")` and `("a", "b")` can't produce the same ID.
- `|` separates the parts, so parts can't run together: repo 12 with PR 345 is `v1|pr|12|345` and repo 123 with PR 45 is `v1|pr|123|45`. A `|` or `%` inside a part, such as a finding's title, is escaped as `%7C` or `%25` first, so a value can't pose as a separator.
- The `v1` prefix lets the scheme change later without clashing with old IDs.
- These IDs are predictable. Never use one as a secret or for access control.
- They aren't time-ordered. Sort by `created_at`.
