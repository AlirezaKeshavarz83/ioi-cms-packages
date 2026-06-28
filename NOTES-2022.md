# IOI 2022 -- Notes

**Source:** Official IOI 2022 TPS packages.

**Host country:** Indonesia (Yogyakarta).

## What was done

Packages were in TPS format. Imported with `cmsImportTask -L tps_task`. Requires the TPS loader Communication patch for the three Communication tasks.

## Task summary

| Task | Type | TL | ML | Subtasks | Notes |
|------|------|----|----|----------|-------|
| circuit | Communication | 2.0s | 2048M | 9 | `std_io`, requires TPS loader patch |
| fish | Batch (grader) | 1.0s | 2048M | 9 | |
| insects | Communication | 2.0s | 2048M | 4 | `std_io`, requires TPS loader patch |
| islands | Batch (grader) | 1.0s | 2048M | 6 | |
| prison | Batch (grader) | 1.0s | 2048M | 4 | |
| towers | Communication | 2.0s | 2048M | 8 | `std_io`, requires TPS loader patch |

## Issues encountered

- **Communication tasks use `std_io`:** All three Communication tasks (`circuit`, `insects`, `towers`) use `std_io` instead of `fifo_io`. The stock TPS loader hardcodes `fifo_io`, so without the Communication patch, these tasks fail to grade correctly.
