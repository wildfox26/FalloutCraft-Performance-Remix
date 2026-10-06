# FalloutCraft Performance Remix

A performance-focused remix of [FalloutCraft](https://github.com/zeyvu/FalloutCraft), targeting higher Fallout-side FPS by avoiding renderer work that cannot contribute visible pixels.

> **Status: source remix / not a tested release yet.**
>
> This repository keeps the original FalloutCraft/SkyCraft credit and license. The optimization below has passed source-level checks, but it has **not** been built or benchmarked in Fallout 4 in this environment.

## What changed

The first pass adds conservative **16×16×16 Minecraft section frustum culling** to the Fallout renderer.

Before:
- Fallout iterated stored Minecraft sections during every render pass.
- Off-screen sections could still reach shadow-buffer refresh and draw submission.

Now:
- Each section gets a conservative camera-frustum AABB test.
- Off-screen sections are skipped for shadow-buffer refresh and rendering.
- A five-second diagnostic reports visible/stored/culled section counts.

The goal is to reduce CPU-side renderer work and draw submissions without changing visible gameplay.

## Verification

Completed:
- `python3 tools/perf_preflight.py`
- `python3 tools/preprocess.py check`
- NeoForge source preprocessing
- 5,000 randomized AABB/frustum comparisons against a brute-force corner test
- Secret/credential scan

Not completed here:
- Fallout 4 runtime benchmark
- Windows/CommonLibF4 native build
- Minecraft Gradle build
- Melty upload/release

**No measured FPS gain is claimed until the modified plugin is run in Fallout 4.**

## Source

This remix was made from the user-supplied source archive of FalloutCraft. The upstream project is:

https://github.com/zeyvu/FalloutCraft

Original project:
- FalloutCraft by **zeyvu**
- Built on SkyCraft by **chasmlol**
- Original license/attribution preserved

## Files

- `FO4_ModFiles/fo_blocks.cpp` — Fallout-side section visibility optimization
- `docs/sheets/performance_section_culling.json` — design/source-of-truth sheet
- `tools/perf_preflight.py` — sheet/reference preflight
- `MODLOG.md` — detailed change and verification log
- `FalloutCraft-performance-remix.patch` — focused patch against the upstream source

## Next test

Build the Fallout 4 F4SE plugin on Windows, launch FalloutCraft, and compare the same scene with the upstream build. Watch the F4SE log for:

`blocks: section frustum culling <visible> visible / <stored> stored (<culled> culled)`

Then compare FPS before/after and check camera turns, distant sections, and shadow refresh when previously off-screen sections become visible.
