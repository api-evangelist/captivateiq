---
name: Bulk import data worksheet records
description: Locate a data worksheet, import records asynchronously via CSV, and poll the job to completion.
api: openapi/captivateiq-openapi-original.yml
operations: [data_worksheets_list, data_worksheets_records_import_template_retrieve, data_worksheets_records_import_create, jobs_retrieve, data_worksheets_records_list]
generated: '2026-07-18'
method: generated
---

# Bulk import data worksheet records

Load large volumes of source data into a CaptivateIQ data worksheet without hitting the 30-second synchronous timeout.

## Auth
`Authorization: Token <your token value>`. Base URL `https://api.captivateiq.com/ciq/v1/`.

## Steps
1. **Find the worksheet** — `GET /ciq/v1/data-worksheets/` (`data_worksheets_list`). Note the target worksheet UUID.
2. **Get the template** — `GET .../records/import/template/` (`data_worksheets_records_import_template_retrieve`) to align your CSV columns. When escaping commas, wrap the field in quotation marks: `"Foo, bar",baz`.
3. **Start the import** — `POST .../records/import/` (`data_worksheets_records_import_create`) with the CSV. This runs **asynchronously** and returns a job reference — the recommended path for large volumes.
4. **Poll the job** — `GET /ciq/v1/jobs/{id}/` (`jobs_retrieve`) until the job completes; check `jobs_list` for status filtering.
5. **Confirm** — `GET .../records/` (`data_worksheets_records_list`) to spot-check imported rows.

## Conventions & errors
- Prefer async import over a large synchronous `POST` — a request over 30s returns `502`.
- `429` on rate-limit; back off. Pagination `limit`/`offset`.
- Batch sizes ~1,000 rows are efficient; validation cost grows with worksheet size.
