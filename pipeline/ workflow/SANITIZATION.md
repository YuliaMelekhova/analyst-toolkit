# Sanitising an n8n Workflow Export

A checklist for publishing an n8n workflow without leaking anything. Written for this repository's pipeline export, but the field inventory applies to any n8n workflow going public.

Work through it in order. Section 1 is the part people get wrong.

## The mistake this exists to prevent

n8n does not export credential secrets. The token, the password and the private key stay in the instance. This is true, it is widely known, and it is the reason exports get published without review.

What n8n *does* export is everything a secret points at: which workspace, which channel, which database, which space, which host, which account. Plus the workflow author's pinned test data, which is real data more often than not.

The realistic threat is not that someone steals a token from the file. It is that the file tells an outsider your Slack workspace ID, your Notion database ID, your Confluence base URL and the name of an internal project that has not been announced, and that one pinned test payload contains a real merchant name and a real email address. None of that is a credential. All of it is a disclosure.

Sanitise for identifiers and context, not just for secrets.

## 1. Field inventory

Every location in an n8n export JSON that can carry something you do not want published. Severity is about likely impact, not likelihood of presence.

### Credentials and auth

| Location | What is there | Severity | Action |
|---|---|---|---|
| `nodes[].credentials.<type>.id` | Internal credential record ID | Medium | Replace with placeholder |
| `nodes[].credentials.<type>.name` | Human-chosen name, frequently `"Slack - Acme Prod"` | High | Replace with generic name |
| `nodes[].parameters` auth fields | Basic-auth users, API keys pasted into HTTP Request nodes instead of credentials | Critical | Remove; this is the one real secret leak |
| `nodes[].parameters.headerParameters` | `Authorization`, `X-Api-Key` header values | Critical | Remove value, keep header name |
| `nodes[].parameters.url` query strings | Tokens passed as query parameters | Critical | Strip query string |

Credential *names* are the routine leak. A credential called `Notion - LumaPay internal` publishes the client relationship regardless of what the token contains.

### Endpoints and instance identity

| Location | What is there | Severity | Action |
|---|---|---|---|
| `meta.instanceId` | Identifier of your n8n instance, stable across exports | Medium | Delete the key |
| `nodes[].webhookId` | UUID of the live webhook path | High | Replace with placeholder UUID |
| `nodes[].parameters.path` | Webhook path segment, often guessable-adjacent | High | Replace |
| `nodes[].parameters.url` | Full internal hostnames, private IPs, `*.atlassian.net`, ngrok tunnels | High | Replace with `https://example.invalid/...` |
| `nodes[].parameters.baseUrl` | Self-hosted service base URLs | High | Replace |

### Platform identifiers

| Location | What is there | Severity | Action |
|---|---|---|---|
| Slack `channelId`, `channel` | `C0…` channel IDs, `T0…` workspace IDs | High | Replace with placeholder in the same shape |
| Notion `databaseId`, `pageId` | UUIDs pointing at real workspaces | High | Replace with a fixed dummy UUID |
| Confluence `spaceKey`, `spaceId`, `parentId` | Space and page identifiers | High | Replace |
| Database nodes: `host`, `database`, `schema`, `port` | Connection targets | High | Replace |
| Google/Drive/Sheets IDs | File and folder identifiers | High | Replace |

Keep the *shape* of each identifier. A Slack channel placeholder that still looks like `C00000000000` keeps the JSON valid for anyone reading it, and makes it obvious it is a placeholder. A UUID replaced with the string `"REDACTED"` breaks schema validation on import and teaches the reader nothing.

### Human identity

| Location | What is there | Severity | Action |
|---|---|---|---|
| `nodes[].parameters` recipient fields | Email addresses, Slack user IDs, mentions | High | Replace with role placeholder |
| Approval and assignment nodes | Named approvers | High | Replace with role name |
| `nodes[].notes` | Author's working notes, frequently mentioning colleagues | Medium | Rewrite or remove |
| `nodes[].name` | Node names like `Send to Maria for sign-off` | Medium | Rename to convention |
| `tags[]`, `versionId`, `createdAt`, `updatedAt` | Workflow provenance | Low | Remove `tags` if internal; timestamps are harmless |

Replace people with roles, not with fake people. `Compliance Reviewer` is informative. `jane.doe@example.com` reads as a real address to a scanner and to a hurried reader.

### Sample and runtime data

| Location | What is there | Severity | Action |
|---|---|---|---|
| `pinData` | Pinned execution data, that is, real payloads from real runs | Critical | Delete the whole key, then rebuild synthetic pins if useful |
| `staticData` | Persisted workflow state, poll cursors, last-seen IDs | High | Delete the key |
| `nodes[].parameters.jsonData` and inline defaults | Hand-pasted example payloads | High | Replace with synthetic examples |
| Code node bodies | Hardcoded IDs, mappings, internal comments | High | Read line by line |

`pinData` is the single highest-yield field in the file for anyone looking. It is invisible in the n8n canvas unless you look for the pin icon, it is preserved by export, and it contains whatever came out of a real execution when the author was debugging. Delete it by default and reintroduce synthetic pins deliberately.

### Prompts and instructions

| Location | What is there | Severity | Action |
|---|---|---|---|
| LLM node system and user prompts | Company name, product names, internal terminology, unreleased features | High | Rewrite against the published context |
| Few-shot examples inside prompts | Real request text, real customer scenarios | Critical | Replace with synthetic examples |
| Referenced knowledge pack names or URLs | Internal wiki structure | Medium | Replace |

Prompt text is prose written for internal readers and is the least likely part of the export to have been reviewed by anyone. Treat it as a document, not as configuration.

### Error handling and logging

| Location | What is there | Severity | Action |
|---|---|---|---|
| Error workflow references | IDs of other internal workflows | Low | Replace |
| Alerting nodes | On-call channels, PagerDuty routing keys, escalation emails | High | Replace |
| Custom error messages | Internal system names and runbook URLs | Medium | Rewrite |

## 2. Placeholder convention

One convention, applied everywhere, so a reader can find every substitution point with a single search.

- **Shape-preserving placeholders** where the JSON schema cares: `C00000000000`, `00000000-0000-0000-0000-000000000000`, `https://example.invalid/webhook/…`.
- **Angle-bracket tokens** where free text is fine: `<<SLACK_CHANNEL_ID>>`, `<<NOTION_DATABASE_ID>>`, `<<CONFLUENCE_SPACE_KEY>>`.
- Every token appears in `env.example` with a one-line description of what to put there.

Placeholders must fail loudly. A sanitised export that imports and runs against a plausible-looking dummy endpoint is worse than one that errors on the first node, because the first behaviour hides the fact that configuration is still needed. Never substitute a real-looking value from a different environment, and never point a placeholder at a domain you do not control. `example.invalid` is reserved by RFC 2606 and will never resolve.

## 3. Procedure

1. Export from n8n. Use **Download**, not clipboard copy; clipboard copy omits some keys in some versions, which sounds helpful and just means you are sanitising a different file than the one you publish.
2. Work on a copy. Keep the original outside the repository and outside any directory you might later `git add`.
3. Delete whole keys first: `pinData`, `staticData`, `meta.instanceId`, `tags`.
4. Walk the node array top to bottom against section 1. Do not search-and-replace globally as a first pass; you will miss identifiers embedded inside expression strings and Code node bodies.
5. Run the verification scans in section 4.
6. Re-import the sanitised file into a scratch n8n instance. It should import cleanly and fail at execution on missing configuration. If it executes successfully, something real is still in there.
7. Only then move the file into the repository.

## 4. Verification

Automated scans catch the mechanical cases. They do not catch an internal project name, so section 5 still applies.

```bash
# Keys that should not exist at all
jq 'has("pinData"), has("staticData"), .meta.instanceId' n8n-workflow.sanitized.json

# Every credential name still present in the file
jq -r '.nodes[] | select(.credentials) | .credentials | to_entries[] | .value.name' \
  n8n-workflow.sanitized.json | sort -u

# Every URL in the file
jq -r '.. | strings | select(test("https?://"))' n8n-workflow.sanitized.json | sort -u

# Common token shapes and platform identifiers
grep -nE 'xox[baprs]-|ntn_|secret_[A-Za-z0-9]{20,}|ATATT[A-Za-z0-9]{20,}|AKIA[0-9A-Z]{16}|ghp_[A-Za-z0-9]{36}' \
  n8n-workflow.sanitized.json

# Email addresses
grep -nE '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}' n8n-workflow.sanitized.json

# UUIDs that are not the dummy value
grep -oE '[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}' \
  n8n-workflow.sanitized.json | sort -u | grep -v '^00000000-'

# Slack identifier shapes that are not placeholders
grep -oE '\b[CTUGD][A-Z0-9]{8,}\b' n8n-workflow.sanitized.json | sort -u | grep -v '0\{6,\}'

# Private and internal hosts
grep -nE '\b(10|127|192\.168)\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\b|\.internal\b|\.local\b|\.atlassian\.net|ngrok' \
  n8n-workflow.sanitized.json
```

Each scan should return nothing, or only values you can point at in `env.example`.

Git history is not covered by any of this. If an unsanitised export was ever committed, sanitising the working file does not remove it. Check with `git log --all --full-history -- 'pipeline/workflow/*'` before assuming the repository is clean, and treat any exposed credential as burned and rotate it rather than trying to rewrite history quietly.

## 5. Manual read

Two things no scan will find, both requiring a human read of the whole file:

**Context that is sensitive without being identifying.** A node named `Route to LatAm expansion review` discloses a business direction. A routing condition on `amount > 50000` discloses a threshold. A branch handling a named partner discloses the partnership. Read the workflow as if you were a competitor, then as if you were the client's legal team.

**Structure that reveals what was omitted.** A gap in node numbering, a disconnected branch, a condition referencing a status value that appears nowhere else. These say a stage was removed and invite the question of what it did.

## 6. Residual risk

State what remains rather than implying the file is clean.

- The workflow structure itself is disclosed: which platforms are integrated, in what order, with what error handling. This is intentional for a published reference implementation and would not be acceptable for an internal system with a security-by-obscurity assumption.
- Node parameter *shapes* reveal which API surface and version is in use, which narrows the version window an outsider would probe against.
- Prompt structure discloses the drafting approach, including the negative constraints, which is exactly what a reader interested in bypassing them would want. Accepted here because the pipeline's control is the human approval gate rather than prompt secrecy. A design that depended on prompt confidentiality would not survive publication and should not be published.

## 7. Sign-off

Do not publish until all of these are true.

- [ ] `pinData`, `staticData`, `meta.instanceId` absent
- [ ] No credential name references an organisation, client or environment
- [ ] No inline auth values in `parameters` or headers
- [ ] All URLs point at `example.invalid` or a documented placeholder
- [ ] All platform identifiers are shape-preserving placeholders
- [ ] No email addresses, personal names or user IDs; roles only
- [ ] Prompts contain no internal terminology, few-shot examples are synthetic
- [ ] Node names and notes follow the repository convention and mention no people
- [ ] All section 4 scans return clean or explained results
- [ ] Whole-file manual read completed against section 5
- [ ] Sanitised file imports into a scratch instance and fails on missing configuration
- [ ] `git log` checked for prior unsanitised commits of the same path
- [ ] Every placeholder token is documented in `env.example`
