## Version notes
- Renv based on R version 4.4.2 with downgraded packages (MASS to 7.3-60.0.1, Matrix to 1.6-5 and mcgv to 1.9-1)
- Posit connect version: 4.2.3.
- Note: check compatibility with 4.2.3 when adding new packages, both for the package itself and dependencies.


## Instructions for r environment
How to get the same r environment:

**1. Open the seamsless_environment via the `.Rproj` file, not by opening a loose `.Rmd`.**  
Double-click `seamless_environment.Rproj` (or _File → Open Project_ in RStudio). This is what triggers `.Rprofile` → `source("renv/activate.R")`, which activates renv for that R session — pointing `.libPaths()` at this project's isolated `renv/library` instead of your personal library. If you just open `Lecture 1.Rmd` directly without opening the project, renv never activates and none of this applies (this is exactly the bug we ran into earlier).

**2. Let renv bootstrap itself if needed.**  
`renv/library` isn't committed to git (`vcs.ignore.library: true` in `renv/settings.json`) — only `renv.lock`, `renv/activate.R`, and `renv/settings.json` are tracked. On first activation, if `renv` itself isn't already installed on their machine, `activate.R` downloads and installs it automatically into a shared renv cache.

**3. Restore the exact pinned package versions:**

renv::restore()

This reads `renv.lock` and installs every package at the exact version recorded there (including archive-pulled versions like `MASS 7.3-60.0.1`) into the project's local `renv/library`. Requires network access, and for source-only packages with compiled code (like `MASS`), a working toolchain — Rtools on Windows, Xcode CLT on macOS, matching their installed R version.

**4. Make sure their R version matches what's pinned.**  
`renv.lock` records the R version the project was built against (`"Version": "4.4.2"` here). `renv::restore()` will warn on a mismatch but won't install R itself — they need R 4.4.x installed separately (e.g. from CRAN, or via `rig`).

**5. Verify everything's consistent:**

renv::status()

Should report the project is in sync with the lockfile with no issues.
