# Contributing to Integration Tracking

Thank you for helping expand You.com API integrations into the open-source ecosystem.

## Requesting Triage Permissions

To file and manage integration issues, you need **Triage** permissions on this repository.

To request access:

1. Open an issue titled `[Access Request] <your GitHub username>`
2. Tag or mention repository maintainers
3. Include:
   - Your intended approach (manual contributions, bot-driven, etc.)
   - Which API(s) you plan to integrate
   - A brief example of a target repo you've identified (optional)

A maintainer will review and grant permissions.

## Ground Rules

- **Claim before you code**: Always file a tracking issue and self-assign before starting work in a target repo. This prevents duplicate effort.
- **One issue per integration**: If a target repo needs multiple APIs, use one issue per API or one issue with multiple `api:*` labels if the integration is tightly coupled.
- **Keep checkboxes updated**: The checklist in the issue body is the async signal to other contributors. Update it when you file upstream issues or open PRs.
- **Be respectful upstream**: Review the target repo's contributing guidelines before opening PRs. You represent You.com in the broader OSS community.
- **Identify your traffic**: Integration code must send the standard `User-Agent` (`youdotcom-integration/<owner>-<repo>`) on every You.com API call so traffic can be attributed. See `skills/integration-tracking/SKILL.md`.
- **Abandon gracefully**: If you can no longer work on an integration, unassign yourself and change the label to `status:stale`, or let the stale bot handle it.

## Label Reference

All labels are managed automatically by `.github/workflows/bootstrap-labels.yml`. Do not create ad-hoc labels unless discussed with maintainers.
