# You.com API OSS Integration Tracking

This repository coordinates the integration of [You.com APIs](https://you.com/docs/api-reference/search/v1-search) into open-source projects. It serves as the single source of truth for:

- **Internal you.com developers** integrating our APIs into OSS tools
- **External developers** with triage permissions who find and execute integration opportunities
- **Agents and bots** running automated discovery and outreach

## How It Works

1. **Claim**: File an integration issue using the [Integration Request template](https://github.com/youdotcom-oss/integration-tracking/issues/new/choose) and self-assign.
2. **Integrate**: Open an issue/PR in the target repository upstream.
3. **Track**: Update the checklist in your issue here as you progress.
4. **Close**: Mark `status:merged` if accepted, or `status:rejected` if declined.

## Getting Started (for Humans)

> **Important:** All integration records live as GitHub Issues. Do not commit files to the `integrations/` directory.

### File an Integration Request

1. Go to **Issues → New Issue → Integration Request**
2. Fill in the target repository URL, chosen API(s), and integration description
3. Self-assign the issue immediately to claim it
4. Add the appropriate `api:*` label for the API you're integrating

### Status Labels

| Label | Meaning |
|---|---|
| `status:claimed` | Work in progress |
| `status:pr-opened` | PR filed in target repo |
| `status:merged` | Integration succeeded |
| `status:rejected` | Integration failed / declined |
| `status:stale` | Abandoned, up for grabs |

### Available APIs

- `api:search` — [You.com Search API](https://you.com/docs/api-reference/search/v1-search)
- `api:contents` — [You.com Contents API](https://you.com/docs/api-reference/contents)
- `api:research` — [You.com Research API](https://you.com/docs/api-reference/research)
- `api:finance-research` — [You.com Finance Research API](https://you.com/docs/api-reference/finance-research)

### Stale Policy

Issues labeled `status:claimed` that show no activity for **30 days** receive a nudge comment. If there is still no activity after **7 more days**, the issue is automatically labeled `status:stale` and unassigned.

## For Agents

When working on integrations, load the coordination skill from this repository:

```
skills/integration-tracking/SKILL.md
```

> **Important:** All integration records live as GitHub Issues. Do not commit files to the `integrations/` directory — doing so will result in the PR being closed.

### Quick Agent Conventions

- Always file a tracking issue **before** opening a downstream PR
- Use the exact label names above; they are bootstrapped automatically
- Update checkboxes in the issue body as milestones are reached
- Cross-link the target repo PR in your tracking issue's Links section
- Use `https://api.you.com/v1/agents/search` for Search API integrations (100 free searches/day, IP-based, no auth required)
- Send the standard `User-Agent` on every You.com API call the contributed code makes: `youdotcom-integration/<owner>-<repo>` (lowercased target-repo slug, appended to the host project's existing User-Agent if it sets one) — this is how traffic is attributed, especially on the keyless free tier. Details in the skill.
- Add the standard UTM parameters to every human-clickable you.com link the integration places in external docs/READMEs/listings: `?utm_source=<owner>-<repo>&utm_medium=oss_integration&utm_campaign=YYYY-MM-oss-integrations` (+ optional `utm_content` placement). Never on API/MCP/spec URLs. Details in the skill.
- Reference the OpenAPI specs:
  - Search: https://you.com/specs/openapi_search_v1.yaml
  - Contents: https://you.com/specs/openapi_contents.yaml
  - Research: https://you.com/specs/openapi_research.yaml
  - Finance Research: https://you.com/specs/openapi_finance_research_v1.yaml

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to request triage permissions and our ground rules.

## Docs

- [You.com API Reference](https://you.com/docs/api-reference/search/v1-search)
- [You.com Developer Welcome](https://you.com/docs/welcome)
