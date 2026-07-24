---
name: Onboard an employee into CaptivateIQ
description: Create an employee (optionally linking or creating a user), verify it, and list to confirm.
api: openapi/captivateiq-openapi-original.yml
operations: [employees_create, employees_retrieve, employees_list, employees_update]
generated: '2026-07-18'
method: generated
---

# Onboard an employee into CaptivateIQ

Use the CaptivateIQ ciq/v1 API to add a sales rep as an employee so they can be paid.

## Auth
Set `Authorization: Token <your token value>` on every request (generate the token in the app under profile settings > API Tokens). Base URL: `https://api.captivateiq.com/ciq/v1/`.

## Steps
1. **Create the employee** — `POST /ciq/v1/employees/` (`employees_create`). Required: `employee_id` (your unique id). Provide `email`, `first_name`, `last_name` unless you pass `linked_user`. Set `is_active: true` for the rep to count toward payouts. Optionally pass `user_settings`, `timezone`, `locale`. Response `201` returns the created `Employee` with its UUID `id`.
2. **Verify** — `GET /ciq/v1/employees/{id}/` (`employees_retrieve`) using the returned UUID; confirm `linked_user` was created/linked.
3. **Confirm in the roster** — `GET /ciq/v1/employees/` (`employees_list`), filter with `?employee_id=` / `?first_name=` / `?last_name=`. The response is a paginated envelope `{object, total_count, next, previous, data}`.
4. **Update if needed** — `PUT /ciq/v1/employees/{id}/` (`employees_update`) to correct details or set `final_process_date`.

## Conventions & errors
- Pagination is `limit`/`offset`; order with `?ordering=employee_id,-last_name`.
- `401` = bad/missing token; `429` = rate limit (Standard 5 rps / 1500 rph); deactivate rather than delete departing reps.
- No idempotency key — do not blindly retry a `POST`; re-check with `employees_list` before recreating.
