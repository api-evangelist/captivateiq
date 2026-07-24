---
name: Export a payout summary for a period
description: Select a payout snapshot's period group and export payout summaries and calculation worksheets.
api: openapi/captivateiq-openapi-original.yml
operations: [payouts_period_groups_list, payouts_payout_summaries_export_create, payouts_worksheets_list, payouts_worksheets_export_create, payouts_payout_dates_list]
generated: '2026-07-18'
method: generated
---

# Export a payout summary for a period

Pull finalized commission payout data out of a CaptivateIQ payout snapshot for reconciliation or downstream payroll.

## Auth
`Authorization: Token <your token value>`. Base URL `https://api.captivateiq.com/ciq/v1/`.

## Steps
1. **List period groups** — `GET .../payouts/period-groups/` (`payouts_period_groups_list`) to find the snapshot's target period group.
2. **(Optional) List payout dates** — `payouts_payout_dates_list` to scope the export to specific dates.
3. **Export payout summaries** — `POST .../payouts/payout-summaries/export/` (`payouts_payout_summaries_export_create`) to generate the payout summary export.
4. **List calculation worksheets** — `GET .../payouts/worksheets/` (`payouts_worksheets_list`) to find the underlying calculation workbook worksheets.
5. **Export a worksheet** — `POST .../payouts/worksheets/export/` (`payouts_worksheets_export_create`) for the detailed calc export.

## Conventions & errors
- Exports may run asynchronously and return downloadable files (CSV); large exports risk the 30s `502` timeout — scope them down or poll a job.
- Pagination `limit`/`offset`; `429` on rate-limit; `401` on bad token.
- Payout data is sensitive commission information — handle exports per the sharing cautions in the docs.
