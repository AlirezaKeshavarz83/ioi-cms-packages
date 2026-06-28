# IOI 2018 -- Notes

**Source:** Official IOI 2018 packages from ioi2018.jp (`ioi2018tests.zip`).

**Host country:** Japan (Tsukuba). Packages had raw test data and graders but were not in TPS format.

## What was done

The raw packages contained test data, graders, and headers, but not in TPS-loader format. A build script (`build_ioi2018.py`) assembled them into the standard package structure:

1. **Test organization:** Tests were originally flat-numbered. Mapped to subtask-prefixed names (`SS-NN.in`/`.out`) using the subtask definitions from the official problem statements (verified against DMOJ).

2. **Subtask definitions:** Manually extracted from the official IOI 2018 statements and cross-referenced with test data characteristics and DMOJ listings.

3. **Checkers written (combo, doll, highway):** Three interactive tasks (combo, doll, highway) needed custom testlib checkers. The graders judge solutions internally and output a verdict line to stdout (e.g. `"Accepted: N"` or `"Wrong Answer: MSG"`). The checkers parse this verdict and apply the partial-scoring formulas from the official problem statements.

4. **Graders:** Copied directly from the official packages. One fix required for doll (see below).

5. **Statements:** Official PDFs from ioi2018.jp.

6. **Contestant attachments:** Official `.zip` packages from ioi2018.jp copied into `attachments/`.

7. **Solutions:** Sourced from IOI editorial author (Tomasz Idziaszek's algonotes), koosaga's olympiad repo (IOI gold medalist), and oj.uz. Mix of full-score and partial solutions for each task.

## Task summary

| Task | Type | TL | ML | Tests | Subtasks | Scoring | Notes |
|------|------|----|----|-------|----------|---------|-------|
| combo | Batch (grader) | 1.0s | 256M | 160 | 3 | Checker (partial on sub2) | Partial scoring based on query count |
| seats | Batch (grader) | 3.0s | 256M | 71 | 7 | White-diff | |
| werewolf | Batch (grader) | 4.0s | 512M | 54 | 5 | White-diff | |
| doll | Batch (grader) | 1.0s | 256M | 56 | 7 | Checker (partial on sub5-6) | Partial scoring based on switch count |
| highway | Batch (grader) | 1.5s | 256M | 126 | 7 | Checker (partial on sub6) | Partial scoring based on query count |
| meetings | Batch (grader) | 4.5s | 768M | 48 | 6 | White-diff | |

## Issues encountered

- **Doll grader file I/O in sandbox:** The official doll grader wrote debug output to `log.txt` and `out.txt` via `fopen()`. In the CMS sandbox (isolate), `fopen()` returns NULL (file creation denied), causing `fprintf(NULL, ...)` -> SIGSEGV, or the log file exceeds isolate's file size limit -> process killed. Fixed by removing all `fopen`/`fprintf`/`fclose` calls for debug files. The verdict still goes to stdout.

- **Doll checker missing P read:** After fixing the grader, solutions got "Wrong Answer: Extra information in the output file". The grader outputs `"Accepted: S P"` (switch count and some value P), but the checker only read S, leaving P unread. Testlib's auto-`seekEof()` found the unread P and converted `_ok` to `_dirt` -> Wrong Answer. Fixed by adding `int P = ouf.readInt()` in the checker (P is consumed but not used in scoring).

- **Seats subtask definitions:** Initially used incorrect subtask constraints. Corrected by cross-referencing with DMOJ and official statement PDFs.
