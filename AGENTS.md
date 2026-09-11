# GitHub Actions billing freeze (2026-09-11)

**Status:** ACTIVE — conservative GitHub usage; **no paid Actions overage**.

Owner (Jon / nelsen3000) incurred unexpected GitHub Actions charges (~$250). Until further notice:

1. **Do not** add new GitHub Actions workflows without explicit owner approval.
2. **Do not** use `macos-*`, `windows-*`, or larger hosted runners. **ubuntu-latest only**.
3. **Do not** add `schedule:` / cron triggers.
4. **Do not** use broad OS/version matrices; avoid artifact spam and unbounded caches.
5. Prefer **local tests** and box/CI-free verification. No `gh workflow run` / re-run unless owner asks.
6. If CI is required later: `timeout-minutes: ≤ 10`, `concurrency` with `cancel-in-progress: true`, and **paths** filters.
7. Prefer disabling risk by renaming workflows to `*.yml.disabled` or removing schedule triggers — not by adding more CI.
8. No CloudAgent / on-demand Actions burn for exploratory work.
9. Owner Billing UI: set Actions spending limit / budget to **$0**.

---
