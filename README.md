# IOI Task Packages for CMS 1.5.1

Ready-to-import task packages for [CMS](https://github.com/cms-dev/cms) (Contest Management System) v1.5.1, covering **10 years of IOI problems** (2016--2025), totaling **60 tasks**.

## Quick Start

```bash
# 1. Apply the TPS loader patch (one-time, see below for details)
cd /path/to/cms
patch -p1 < tps_loader.patch

# 2. Download and extract the release tarball for the year you want
tar xzf ioi2024-cms151-packages.tar.gz
cd ioi2024-cms151

# 3. Import into CMS (creates contest + all tasks)
./make_contest.sh
```

## Prerequisites

- CMS v1.5.1 with PostgreSQL and isolate
- **TPS loader patch applied** (see below)
- `pyyaml` installed in the CMS virtualenv (`pip install pyyaml`)

### Required CMS Patch: TPS Loader

Two bugs in the stock CMS v1.5.1 TPS loader (`cmscontrib/loaders/tps.py`) must be fixed before importing:

**1. OutputOnly submission format** (affects IOI 2017 `nowruz`, IOI 2019 `line`):

The loader writes `"<codename>.out"` but the OutputOnly task type expects `"output_<codename>.txt"`. Without this fix, every OutputOnly task scores 0 ("File not submitted").

```diff
--- a/cmscontrib/loaders/tps.py
+++ b/cmscontrib/loaders/tps.py
@@ -176,7 +176,7 @@
         if data["task_type"] == 'OutputOnly':
             args["submission_format"] = list()
             for codename in testcase_codenames:
-                args["submission_format"].append("%s.out" % codename)
+                args["submission_format"].append("output_%s.txt" % codename)
```

**2. Communication task type parameters** (affects tasks with `std_io` or custom compilation):

The loader hardcodes `"stub"` and `"fifo_io"` for all Communication tasks, ignoring `problem.json` overrides. Without this fix, Communication tasks that use `std_io` (most 2022--2025 tasks) will fail.

```diff
--- a/cmscontrib/loaders/tps.py
+++ b/cmscontrib/loaders/tps.py
@@ -97,7 +97,17 @@
         if task_type == 'Communication':
             par_processes = '%s_num_processes' % par_prefix
+            par_compilation = '%s_compilation' % par_prefix
+            par_user_io = '%s_user_io' % par_prefix
             if par_processes not in task_type_parameters:
                 task_type_parameters[par_processes] = 1
-            return [task_type_parameters[par_processes], "stub", "fifo_io"]
+            if par_compilation not in task_type_parameters:
+                task_type_parameters[par_compilation] = 'stub'
+            if par_user_io not in task_type_parameters:
+                task_type_parameters[par_user_io] = 'std_io'
+            return [
+                task_type_parameters[par_processes],
+                task_type_parameters[par_compilation],
+                task_type_parameters[par_user_io],
+            ]
```

A unified patch file (`tps_loader.patch`) combining both fixes is included in the repo under `patches/` and as a standalone file in each release tarball. Apply it with:

```bash
cd /path/to/cms
patch -p1 < tps_loader.patch
```

## Releases

Each release contains a single tarball with all 6 tasks for that year's IOI, plus import scripts.

| Year | Tasks | Notes |
|------|-------|-------|
| [IOI 2016](../../releases/tag/ioi2016) | aliens, messy, molecules, paint, railroad, shortcut | [NOTES-2016.md](NOTES-2016.md) |
| [IOI 2017](../../releases/tag/ioi2017) | books, nowruz, prize, simurgh, train, wiring | [NOTES-2017.md](NOTES-2017.md) |
| [IOI 2018](../../releases/tag/ioi2018) | combo, doll, highway, meetings, seats, werewolf | [NOTES-2018.md](NOTES-2018.md) |
| [IOI 2019](../../releases/tag/ioi2019) | line, rect, shoes, split, vision, walk | [NOTES-2019.md](NOTES-2019.md) |
| [IOI 2020](../../releases/tag/ioi2020) | biscuits, mushrooms, plants, stations, supertrees, tickets | [NOTES-2020.md](NOTES-2020.md) |
| [IOI 2021](../../releases/tag/ioi2021) | candies, dna, dungeons, keys, parks, registers | [NOTES-2021.md](NOTES-2021.md) |
| [IOI 2022](../../releases/tag/ioi2022) | circuit, fish, insects, islands, prison, towers | [NOTES-2022.md](NOTES-2022.md) |
| [IOI 2023](../../releases/tag/ioi2023) | beechtree, closing, longesttrip, overtaking, robot, soccer | [NOTES-2023.md](NOTES-2023.md) |
| [IOI 2024](../../releases/tag/ioi2024) | hieroglyphs, message, mosaic, nile, sphinx, tree | [NOTES-2024.md](NOTES-2024.md) |
| [IOI 2025](../../releases/tag/ioi2025) | festival, migrations, obstacles, souvenirs, triples, worldmap | [NOTES-2025.md](NOTES-2025.md) |

## Package Format

Each task directory follows a standard structure:

```
task_name/
  problem.json          # Task metadata (type, TL, ML, scoring, submission format)
  tests/                # Test inputs (.in) and expected outputs (.out)
  subtasks/             # Subtask definitions (JSON: score + testcase list)
  graders/              # Grader source (grader.cpp + task.h) for interactive/grader tasks
  checker/              # Custom checker (checker.cpp + testlib.h) where needed
  solutions/
    model_solution/     # Reference solution (may be empty)
    correct/            # Full-score solutions
    partially_correct/  # Partial-score solutions
  statements/           # Official problem statement PDF (en_US.pdf)
  attachments/          # Contestant package (task.zip with sample grader + headers)
```

Each tarball also includes:
- `contests.yaml` -- contest definitions (Day 1 / Day 2) with language config
- `make_contest.sh` / `make_contest.py` -- import script that creates contest(s) and imports all tasks
- `IMPORT.md` -- quick-start instructions

## Importing

The `make_contest.sh` script handles everything:

1. Creates the contest(s) defined in `contests.yaml`
2. Imports each task with `cmsImportTask -L tps_task`
3. Assigns tasks to their respective contest (Day 1 or Day 2)

To re-import a single task after changes:
```bash
source ~/.cms_venv/bin/activate
python3 -m cmscontrib.ImportTask --update-task --task-name <task_name> path/to/<task_name>/
```

## Submitting Solutions

```bash
source ~/.cms_venv/bin/activate
python3 -m cmscontrib.AddSubmission <username> <task_name> \
    -c <contest_id> \
    -f "<task_name>.%l:path/to/solution.cpp"
```

Evaluation happens automatically if `cmsResourceService` is running.

## Sources

- **Test data and graders:** Official IOI packages from [ioinformatics.org](https://ioinformatics.org), IOI host country websites, and [TPS](https://github.com/ioi-2017/tps) repositories
- **Problem statements:** Official PDFs from [stats.ioinformatics.org](https://stats.ioinformatics.org) and IOI host websites
- **Checkers:** [testlib.h](https://github.com/nicedude11/testlib) (CMS-compatible fork, v0.9.12)

## License

The test data, problem statements, and graders are the intellectual property of the respective IOI host countries and scientific committees. This repository redistributes them for educational and training purposes.
