## Version notes
- Renv is based on R version 4.4.2 with downgraded packages (MASS to 7.3-60.0.1, Matrix to 1.6-5 and mcgv to 1.9-1)
- Posit connect version: 4.2.3 -- source of potential compatibility issues that are currently handled in the renv.
- Note: Check compatibility with 4.2.3 when adding new packages, both for the package itself and dependencies.
- Note: Running a lecture .rmd (run document in Rstudio) locally automatically regenerates the manifest.json file for that lecture; this is how updates/changes in packages are recorded. If the local environment is not compatible with the server then keeping the manifest version in git and discarding local changes to manifest.json might be a workaround.

## General notes
 
- Note: Loading will be faster for students (and you if you do it early enough) if you set min processes to 1 or more before the start of the lecture in the relevant lecture app, but it is best to set it back to 0 after the lecture is over, so past lectures don't stay active in the background taking up RAM. (Content -> open lecture -> gear icon -> runtime -> min processes : 1 -> save); if students still experience slow loading you can try 2-4. This is basically how many processes are preheated before anyone requests them. 4 accommodates the max amount of ~100 students.

## Instructions for local r environment
How to get the same r environment:

**1. Open the seamsless_environment via the `.Rproj` file, not by opening a loose `.Rmd`.**  
Double-click `seamless_environment.Rproj` (or _File → Open Project_ in RStudio). This is what triggers `.Rprofile` → `source("renv/activate.R")`, which activates renv for that R session — pointing `.libPaths()` at this project's isolated `renv/library` instead of your personal library. If you just open `Lecture 1.Rmd` directly without opening the project, renv never activates and none of this applies (this is exactly the bug we ran into earlier).

**2. Let renv bootstrap itself if needed.**  
`renv/library` isn't committed to git (`vcs.ignore.library: true` in `renv/settings.json`) — only `renv.lock`, `renv/activate.R`, and `renv/settings.json` are tracked. On first activation, if `renv` itself isn't already installed on the machine, `activate.R` downloads and installs it automatically into a shared renv cache.

**3. Restore the exact pinned package versions:**

renv::restore()

This reads `renv.lock` and installs every package at the exact version recorded there (including archive-pulled versions like `MASS 7.3-60.0.1`) into the project's local `renv/library`. Requires network access, and for source-only packages with compiled code (like `MASS`), a working toolchain — Rtools on Windows, Xcode CLT on macOS, matching their installed R version.

**4. Make sure their R version matches what's pinned.**  
`renv.lock` records the R version the project was built against (`"Version": "4.4.2"` here). `renv::restore()` will warn on a mismatch but won't install R itself — they need R 4.4.x installed separately (e.g. from CRAN, or via `rig`).

**5. Verify everything's consistent:**

renv::status()

Should report the project is in sync with the lockfile with no issues.

## Uploading to Posit

https://seamless.uvt.nl/ -> publish - import from Git -> https://github.com/travisjwiltshire/statistics_for_csai_II_cloud/ -> posit2026 - Lecture

Recommended set-up steps:
- Set name (Content -> open lecture -> gear icon -> info -> insert name -> save)
- Set access (Content -> open lecture -> gear icon -> access -> sharing -> all users -> save)
- Set url (Content -> open lecture -> gear icon -> access ->  path -> course/lecture -> save)
- Set runtime settings (Content -> open lecture -> gear icon -> runtime -> max processes : 4, max connections : 25, load factor : 0.75, initial timeout: 120, max ram: 1.5, default settings for rest -> save)
- Set a tag (Content -> open lecture -> gear icon -> tag -> select -> save)

## Updating in Posit 
Content -> open lecture -> gear icon -> info tab -> scroll down -> update now
* It is probably best to uncheck "check for updates periodically" for more control over when updates happen.

## Managing tags in Posit
Tags let you filter by e.g. course on the content page. You can add or edit tags under admin -> tags from the top of the page menu. For new courses, you can add a new tag under that category. You can also add new categories (a separate sub-list with a header in the filter list) using the top right plus button. Tags are assigned to lectures/apps in the app's tag menu (see set-up steps for how to set a tag).



