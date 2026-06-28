# IOI 2025 -- Notes

**Source:** Official IOI 2025 TPS packages.

**Host country:** Bolivia (Santa Cruz de la Sierra).

## What was done

Packages were in TPS format. Imported with `cmsImportTask -L tps_task`. Requires the TPS loader Communication patch for `migrations` and `souvenirs`.

## Task summary

| Task | Type | TL | ML | Subtasks | Notes |
|------|------|----|----|----------|-------|
| festival | Batch (grader) | 1.0s | 2048M | 8 | |
| migrations | Communication (2 processes) | 3.0s | 2048M | 3 | `std_io`, requires TPS loader patch |
| obstacles | Batch (grader) | 2.0s | 2048M | 7 | |
| souvenirs | Communication | 1.0s | 2048M | 7 | `std_io`, requires TPS loader patch |
| triples | Batch (grader) | 2.0s | 2048M | 13 | |
| worldmap | Batch (grader) | 1.0s | 2048M | 7 | |

## Issues encountered

- **Communication tasks use `std_io`:** Both `migrations` and `souvenirs` use `std_io`. Requires the TPS loader patch.
- **`migrations` has 2 processes:** A two-process Communication task.
