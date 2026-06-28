# IOI 2023 -- Notes

**Source:** Official IOI 2023 TPS packages.

**Host country:** Hungary (Szeged).

## What was done

Packages were in TPS format. Imported with `cmsImportTask -L tps_task`.

1. **TPS loader patch required:** `longesttrip` and `robot` are Communication tasks. The stock TPS loader hardcodes Communication parameters, ignoring `problem.json` overrides. The included TPS loader patch ensures the `compilation` and `user_io` parameters are read correctly.

## Task summary

| Task | Type | TL | ML | Subtasks | Notes |
|------|------|----|----|----------|-------|
| beechtree | Batch (grader) | 1.5s | 2048M | 10 | |
| closing | Batch (grader) | 1.0s | 2048M | 10 | |
| longesttrip | Communication | 1.0s | 2048M | 5 | `fifo_io` |
| overtaking | Batch (grader) | 2.5s | 2048M | 6 | |
| robot | Communication | 1.0s | 2048M | 6 | `fifo_io` |
| soccer | Batch (grader) | 3.5s | 2048M | 7 | |

## Issues encountered

None beyond the TPS loader patch requirement.
