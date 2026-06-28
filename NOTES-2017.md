# IOI 2017 -- Notes

**Source:** Official IOI 2017 TPS packages from ioinformatics.org (`ioi2017tests.zip`).

**Host country:** Iran (Tehran). Packages are in native TPS format.

## What was done

Packages were already in TPS format, so the main work was exporting with `tps export CMS 1` (protocol 1, required by CMS v1.5.1) and fixing two incompatibilities:

1. **OutputOnly submission format (nowruz):** The TPS loader writes `"<codename>.out"` but the CMS OutputOnly task type expects `"output_<codename>.txt"`. Without the TPS loader patch (see README), nowruz is ungradable -- every testcase gets "File not submitted" and scores 0. After patching, model solution scores 100/100.

2. **Communication manager protocol (prize):** Prize used the old `Communication2017` two-stage design: the manager interacted with the solution then dumped a query log to stdout, and a separate testlib checker (`checker.cpp`) scored based on query count. Mainline CMS Communication has no checker stage -- the manager alone must print a float outcome `[0,1]` on stdout. CMS died with `ValueError: Outcome is not a float` because the manager printed `"OK"`/`"WA"` + a log.

   Fix: an adapted manager that folds the checker's query-count scoring into the manager (`grade_outcome`) and prints the float outcome to stdout + text to stderr. Also adds `signal(SIGPIPE, SIG_IGN)` so the manager survives when the user program exits first. No CMS source changes required -- this is a data-side fix only.

3. **TPS script overlay:** The published packages ship old TPS scripts without the CMS exporter. Overlaid current `ioi-2017/tps` scripts (equivalent to `tps upgrade-scripts`) and synthesized `tests/gen_summary` from the already-released `.in` files (the exporter requires it).

4. **Statements:** Official PDFs from the IOI 2017 contest site (`ioi2017.ir`).

## Task summary

| Task | Type | TL | ML | Subtasks | Notes |
|------|------|----|----|----------|-------|
| books | Batch (grader) | 2.0s | 1024M | 6 | No changes needed |
| train | Batch (grader) | 2.0s | 256M | 7 | No changes needed |
| wiring | Batch (grader) | 1.0s | 256M | 6 | No changes needed |
| simurgh | Batch (grader) | 2.0s | 1024M | 6 | No changes needed |
| nowruz | OutputOnly | 100.0s | 256M | 10 | Requires TPS loader patch |
| prize | Communication | 1.0s | 1024M | 3 | Adapted manager (see above) |

## Issues encountered

- **OutputOnly loader bug:** Upstream CMS v1.5.1 bug, not specific to 2017 data. Affects any OutputOnly task imported via TPS loader.
- **Communication2017 protocol:** The two-stage (manager + checker) design was a 2017-era CMS extension. Later IOI years use the mainline single-manager protocol. Prize is the only 2017 task affected.
