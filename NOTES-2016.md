# IOI 2016 -- Notes

**Source:** Official IOI 2016 setter packages from ioinformatics.org (`ioi2016tests.zip`).

**Host country:** Russia (Kazan). Scientific committee packages used Polygon/Codeforces format (not TPS).

## What was done

The raw packages were in Polygon format (numbered tests, `problem.xml` groups, Codeforces-style testlib checkers). A build script (`build_ioi2016.py`) converted them to the CMS TPS-loader format:

1. **Test renaming:** Sequential numbering (`01`, `01.a`, `02`, ...) was mapped to subtask-prefixed names (`SS-NN.in`/`.out`) using the group definitions in `problem.xml`.

2. **Subtask extraction:** Test-to-subtask membership was parsed from `problem.xml` `<test-group>` elements and written as subtask JSON files.

3. **Checker migration (testlib v0.9.10 -> v0.9.12):** All 6 checkers used testlib.h v0.9.10 with `registerTestlibCmd(argc, argv)`, which outputs verdicts via exit codes + stderr (Codeforces convention). CMS requires testlib.h v0.9.12 with `registerChecker("taskname", argc, argv)`, which outputs a float score on stdout. Replaced testlib.h and changed the registration call in all 6 `checker.cpp` files.

4. **Graders and headers:** Copied directly from the official packages (no modifications needed).

5. **Statements:** Official PDFs from ioinformatics.org task archive.

6. **Solutions:** Official setter solutions (by Burunduk1, tourist, izban, sk, etc.) with tags MAIN/ACCEPTED/REJECTED. Reorganized into `correct/` and `partially_correct/` based on intended score.

## Task summary

| Task | Type | TL | ML | Tests | Subtasks | Checker | Notes |
|------|------|----|----|-------|----------|---------|-------|
| molecules | Batch (grader) | 1.0s | 256M | 121 | 7 | testlib | Validates subset membership |
| railroad | Batch (grader) | 2.0s | 256M | 93 | 5 | testlib | Zero-check for subtask 1 |
| shortcut | Batch (grader) | 2.0s | 256M | 220 | 9 | testlib | Equality check |
| paint | Batch (grader) | 2.0s | 1024M | 137 | 8 | testlib | Char-by-char compare |
| messy | Batch (grader) | 2.0s | 256M | 76 | 6 | testlib | Interactive verdict from grader |
| aliens | Batch (grader) | 2.0s | 256M | 177 | 7 | testlib | Equality check |

All 6 tasks use testlib checkers because graders embed secret tokens in their output -- white-diff is not possible.

## Issues encountered

- **Testlib incompatibility:** The v0.9.10 checkers output verdicts via exit codes + stderr (Codeforces convention), but CMS expects a float score on stdout. This caused "Invalid output from checker: Outcome is not a float" on every evaluation. Fixed by upgrading to testlib.h v0.9.12 with `registerChecker()`.
