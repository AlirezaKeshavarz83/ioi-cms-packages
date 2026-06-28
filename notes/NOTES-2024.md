# IOI 2024 -- Notes

**Source:** Official IOI 2024 TPS packages.

**Host country:** Egypt (Alexandria).

## What was done

Packages were in TPS format. Imported with `cmsImportTask -L tps_task`. Requires the TPS loader Communication patch for `message` and `sphinx`.

## Task summary

| Task | Type | TL | ML | Subtasks | Notes |
|------|------|----|----|----------|-------|
| hieroglyphs | Batch (grader) | 1.0s | 2048M | 7 | |
| message | Communication (2 processes) | 3.0s | 2048M | 3 | `std_io`, requires TPS loader patch |
| mosaic | Batch (grader) | 1.0s | 2048M | 9 | |
| nile | Batch (grader) | 2.0s | 2048M | 8 | |
| sphinx | Communication | 1.5s | 2048M | 6 | `std_io`, requires TPS loader patch |
| tree | Batch (grader) | 2.0s | 2048M | 8 | |

## Issues encountered

- **Communication tasks use `std_io`:** Both `message` and `sphinx` use `std_io`. Without the TPS loader patch, the loader hardcodes `fifo_io` and these tasks fail.
- **`message` has 2 processes:** A two-process Communication task where the contestant implements both `send_message()` and `receive_message()`.
