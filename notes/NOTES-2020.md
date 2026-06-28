# IOI 2020 -- Notes

**Source:** Official IOI 2020 TPS packages.

**Host country:** Singapore (online due to COVID-19).

## What was done

Packages were in TPS format. Imported with `cmsImportTask -L tps_task`.

1. **TPS loader patch required:** `mushrooms` and `stations` are Communication tasks. The stock TPS loader hardcodes Communication parameters, ignoring `problem.json` overrides. The included TPS loader patch ensures the `compilation` and `user_io` parameters are read correctly.

## Task summary

| Task | Type | TL | ML | Subtasks | Notes |
|------|------|----|----|----------|-------|
| biscuits | Batch (grader) | 1.0s | 2048M | 6 | |
| mushrooms | Communication | 2.0s | 1024M | 2 | `fifo_io` |
| plants | Batch (grader) | 4.0s | 2048M | 8 | |
| stations | Communication (2 processes) | 1.0s | 2048M | 6 | `fifo_io` |
| supertrees | Batch (grader) | 1.0s | 2048M | 7 | |
| tickets | Batch (grader) | 2.0s | 2048M | 8 | |

## Issues encountered

- **`stations` has 2 processes:** A two-process Communication task where the contestant implements both `label()` and `find_station()`.
