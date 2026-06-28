# IOI 2019 -- Notes

**Source:** Official IOI 2019 TPS packages.

**Host country:** Azerbaijan (Baku).

## What was done

Packages were in TPS format. Imported with `cmsImportTask -L tps_task`.

1. **TPS loader patch required:** `line` is an OutputOnly task. The stock CMS v1.5.1 TPS loader writes the wrong submission format (`"<codename>.out"` instead of `"output_<codename>.txt"`), making OutputOnly tasks ungradable. The included TPS loader patch fixes this.

## Task summary

| Task | Type | TL | ML | Subtasks | Notes |
|------|------|----|----|----------|-------|
| line | OutputOnly | 100.0s | 256M | 11 | Requires TPS loader patch (OutputOnly submission format) |
| rect | Batch (grader) | 5.0s | 1024M | 8 | |
| shoes | Batch (grader) | 1.0s | 1024M | 7 | |
| split | Batch (grader) | 2.0s | 1024M | 6 | |
| vision | Batch (grader) | 1.0s | 1024M | 9 | |
| walk | Batch (grader) | 4.0s | 1024M | 6 | |

All tasks use testlib checkers. Solutions from the official setter packages are included.

## Issues encountered

- **`line` is OutputOnly:** The stock TPS loader writes the wrong submission format for OutputOnly tasks, causing every testcase to score 0 ("File not submitted"). The included TPS loader patch fixes this.
