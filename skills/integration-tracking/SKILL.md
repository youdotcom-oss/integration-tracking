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

## APIs and Endpoints

All You.com API integrations authenticate via the `YDC_API_KEY` environment variable. When setting up an integration, configure the target repo/tooling to read the API key from `YDC_API_KEY` (e.g. in CI secrets, `.env` files, or runtime config). Do not hardcode keys or use alternative variable names.

| API | Label | OpenAPI Spec | Notes |
|---|---|---|---|
| Search | `api:search` | https://you.com/specs/openapi_search_v1.yaml | Use `https://api.you.com/v1/agents/search` for 100 free searches/day (IP-based, no livecrawl) |
| Contents | `api:contents` | https://you.com/specs/openapi_contents.yaml | Extract content from URLs |
| Research | `api:research` | https://you.com/specs/openapi_research.yaml | Synthesized answers with citations |
| Finance Research | `api:finance-research` | https://you.com/specs/openapi_finance_research_v1.yaml | Financial data queries |

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
