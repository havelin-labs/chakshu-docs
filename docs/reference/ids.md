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
| org, repo, user, team | `v1\|org\|<github id>` |
| reviewer | `v1\|reviewer\|<org github id>\|<name>` |
| pull request | `v1\|pr\|<repo github id>\|<number>` |
| work key | `v1\|work\|<pr id>\|<head sha>\|<reviewer sha>\|<reviewer id>` |
| finding | `v1\|finding\|<pr id>\|<reviewer id>\|<path>\|<category>\|<title>` |

## Why it helps

- A webhook GitHub delivers twice maps to the same row, so the second one is dropped.
- Two requests for the same reviewer, PR and commit share one review.
- Each posted comment carries its finding ID in a hidden marker, so a re-run doesn't repeat it.
- Two installs never share a namespace (different GitHub App IDs), so exports never collide.

## Rules

- Hash GitHub's numeric IDs, not logins or repo names. Names can be renamed.
- Parts are escaped, so `("a|b")` and `("a", "b")` can't produce the same ID.
- The `v1` prefix lets the scheme change later without clashing with old IDs.
- These IDs are predictable. Never use one as a secret or for access control.
- They aren't time-ordered. Sort by `created_at`.
