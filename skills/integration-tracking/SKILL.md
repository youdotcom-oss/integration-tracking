---
name: integration-tracking
description: >
  Coordination skill for tracking You.com API integrations into open-source repositories.
  Guides agents to file structured issues, maintain status via labels and checkboxes,
  and cross-link downstream PRs. Intended for use with the integration-tracking repo.
---

# Integration Tracking Skill

Use this skill when working on You.com API integrations into OSS repos via the `integration-tracking` repo.

## Conventions

1. **Always file a tracking issue first** before opening any downstream PR or issue.
2. Use the `.github/ISSUE_TEMPLATE/integration-request.yml` form.
3. Self-assign the tracking issue immediately to apply `status:claimed`.
4. Add the appropriate `api:*` label(s) to the tracking issue.
5. Update checkboxes in the issue body as milestones are reached.
6. Cross-link the target repo issue/PR in the tracking issue's Links section.
7. Set the standard integration `User-Agent` on every You.com API call the contributed code makes (see [Outbound User-Agent](#outbound-user-agent-required)).

## APIs and Endpoints

All You.com API integrations authenticate via the `YDC_API_KEY` environment variable. When setting up an integration, configure the target repo/tooling to read the API key from `YDC_API_KEY` (e.g. in CI secrets, `.env` files, or runtime config). Do not hardcode keys or use alternative variable names.

| API | Label | OpenAPI Spec | Notes |
|---|---|---|---|
| Search | `api:search` | https://you.com/specs/openapi_search_v1.yaml | Use `https://api.you.com/v1/agents/search` for 100 free searches/day (IP-based, no livecrawl) |
| Contents | `api:contents` | https://you.com/specs/openapi_contents.yaml | Extract content from URLs |
| Research | `api:research` | https://you.com/specs/openapi_research.yaml | Synthesized answers with citations |
| Finance Research | `api:finance-research` | https://you.com/specs/openapi_finance_research_v1.yaml | Financial data queries |

## Outbound User-Agent (required)

Every HTTP call to a You.com API made by integration code — whether authored by a bot or an engineer — MUST send an identifiable `User-Agent` so You.com can attribute traffic to the integration. On the free `https://api.you.com/v1/agents/search` tier there is no API key, so the User-Agent is the primary attribution signal.

**Format:** `youdotcom-integration/<owner>-<repo>` where `<owner>-<repo>` is the lowercased target repo slug.

| Target repo | User-Agent |
|---|---|
| `BerriAI/litellm` | `youdotcom-integration/berriai-litellm` |
| `open-webui/open-webui` | `youdotcom-integration/open-webui-open-webui` |

Rules:

- If the host project's HTTP client already sends its own meaningful `User-Agent`, **append** our product token (User-Agents are space-separated product tokens) instead of replacing it: `litellm/1.74.0 youdotcom-integration/berriai-litellm`. If only the HTTP library's default would be sent (`python-requests/…`, `axios/…`), set ours outright.
- Apply it **only** to requests to You.com hosts (`api.you.com`, legacy `api.ydc-index.io`) — never to the project's other traffic.
- Optionally append the comment `(+https://github.com/youdotcom-oss/integration-tracking)` if the upstream project has no objection.
- Record the exact User-Agent string in the tracking issue's Notes section.
- You.com's own SDKs identify themselves with their package name and version instead (e.g. `youdotcom-python-sdk/0.3.1`).

The User-Agent attributes **API traffic**. Human-clickable links are attributed separately — see [Outbound UTM Attribution](#outbound-utm-attribution-required-for-youcom-links).

## Outbound UTM Attribution (required for you.com links)

Every human-clickable link to a you.com property (`you.com`, `you.com/platform`, `you.com/docs/...`) that integration work places in an external surface — READMEs, docs pages, marketplace listings, sample configs, upgrade-hint error messages — MUST carry UTM parameters following the you.com marketing taxonomy (2026 UTM Builder and Taxonomy):

| Parameter | Value | Notes |
|---|---|---|
| `utm_source` | `<owner>-<repo>` | Lowercased target repo slug where the link lives — the same slug as the User-Agent convention (e.g. `mastra-ai-mastra`) |
| `utm_medium` | `oss_integration` | Fixed channel value for integration work |
| `utm_campaign` | `YYYY-MM-<initiative>` | Ship month + initiative. Default initiative is `oss-integrations` (e.g. `2026-07-oss-integrations`); use a more specific one only when the work belongs to a named campaign |
| `utm_content` | placement (optional) | Where the link appears: `readme`, `docs`, `marketplace`, `error-message`, `sample` |
| `utm_term` | — | Not used for integrations |

Example:

```
https://you.com/platform?utm_source=mastra-ai-mastra&utm_medium=oss_integration&utm_campaign=2026-07-oss-integrations&utm_content=readme
```

Rules:

- Apply **only** to links a human clicks. Never add UTM parameters to machine-fetched URLs — API endpoints, MCP server URLs, OpenAPI spec URLs, env-var values. Those are attributed by the User-Agent convention instead.
- Append with `&` when the URL already has a query string; keep values lowercase (hyphens inside campaign initiatives, underscores in `utm_medium`).
- Use the ship month of the integration for `YYYY-MM`, and keep one consistent `utm_campaign` across all links in a single integration.
- Record the exact `utm_campaign` (and any non-default values) in the tracking issue's Notes section, like the User-Agent string.
- If the upstream project strips or rejects tracking parameters (some docs linters do), ship bare links rather than fight upstream style, and note that in the tracking issue.

## Issue Lifecycle Commands

When updating a tracking issue, use the checklist and labels in this order:

| Milestone | Action |
|---|---|
| Claimed | Self-assign, label `status:claimed` |
| Upstream issue filed | Check "Filed issue or opened discussion in target repo" |
| Downstream PR opened | Check "Opened PR in target repo", add label `status:pr-opened`, paste PR link in Links section |
| PR merged | Check "PR merged or rejected", change label to `status:merged`, paste final PR link |
| PR rejected | Check "PR merged or rejected", change label to `status:rejected`, paste PR link with note |

## Drafting an Integration Request

When asked to file a new integration, gather:

- `target_repo`: Full GitHub URL of the OSS repo
- `apis`: Array of API names from the table above
- `description`: One-paragraph explanation of what the integration enables
- `complexity`: Estimate from `small`, `medium`, `large`
- `contributing_url` (optional): Link to target repo's CONTRIBUTING.md
- `approach` (optional): High-level technical plan

Then create the issue using the Integration Request template with these values.

## Updating Status

When asked to update progress, do not rewrite the issue body. Instead:
1. Add/edit the Links section with new URLs
2. Check/uncheck the specific checklist items
3. Update labels via the GitHub API or `gh` CLI
4. Add a comment summarizing the milestone

## Stale Policy Awareness

If an issue has been `status:claimed` with no updates for 30+ days, the stale bot will nudge. If the user indicates they are abandoning the work, remove the assignee and apply `status:stale`.
