# FalloutCraft remix log

## 2026-10-06 — performance / higher-FPS pass

- Upstream source: user-supplied FalloutCraft source archive.
- Original credit preserved; no upstream attribution or license files were removed.
- Target: higher Fallout-side FPS by reducing work that cannot contribute visible pixels.
- Chosen route: native Fallout 4 renderer path (`FO4_ModFiles/fo_blocks.cpp`).

### Change

- Added a conservative homogeneous clip-frustum AABB rejection test for 16×16×16 Minecraft sections.
- Reused visibility to skip off-screen per-section shadow-stream updates and draw submissions.
- Added a five-second diagnostic log reporting visible versus stored sections.
- Added a source-of-truth performance sheet and preflight checker.

### Verification completed

- `python3 tools/perf_preflight.py` — passed.
- `python3 tools/preprocess.py check` — passed.
- NeoForge source preprocessing — passed.
- 5,000 randomized plane/AABB comparisons matched a brute-force corner test.
- Secret/credential scan — passed.

### Not completed

- No Fallout 4 runtime test in this environment.
- No Windows/CommonLibF4 native build in this environment.
- No Minecraft Gradle build in this environment.
- No measured FPS gain is claimed.
