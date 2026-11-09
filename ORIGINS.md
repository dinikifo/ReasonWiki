# Origins and related personal projects

This project is an **experimental personal wiki for managing projects and
recording decisions**. According to the author, it grew out of earlier work in
personal GitHub repositories, most notably **Personal Wiki** and **Addendum of
the Desiderata**. Its origins are worth documenting even though the current
release is intended to use CC0.

## Related repositories

### 1. Personal Wiki

- **GitHub repository:** `ADD_PERSONAL_WIKI_GITHUB_URL`
- **Relationship:** the underlying local wiki / notebook approach, including
  SQLite storage, posts, history, and a desktop interface, forms the immediate
  technical starting point for this iteration.
- **This edition adds or develops:** typed field contracts, a project dashboard,
  structured project documentation, argument forms, and an integration path for
  the separate PVS system.

### 2. Addendum of the Desiderata

- **GitHub repository:** `ADD_DESIDERATA_GITHUB_URL`
- **Relationship:** the author identifies this as an additional personal
  project that influenced the experiment. The exact file-level reuse or
  conceptual contributions have not been verified from the materials supplied.
  In this release, the connection is presented as **related work**, not as a
  proven code dependency or a claim about identical functionality.

### 3. PVS-01 (external integration)

The wiki includes an optional workflow to read and reconcile **PVS-01** project
snapshots. PVS remains a separate system: the wiki reads exported data and can
list local status changes that need to be entered in PVS, but it does **not**
write directly to the PVS workspace. No PVS source checkout is packaged here.
If PVS is published separately, its repository link can be added here.

## Provenance and licensing notes

The URL markers above must be replaced with the author's *actual* repository
URLs before publication. The source files supplied for this release do not
include authoritative upstream Git remote metadata or a file-by-file provenance
map. No commit or tag attribution has been invented.

The publisher intends to dedicate their original source, documentation, and
generated examples in this repository to the public domain under **CC0 1.0
Universal** (`LICENSE`), to the extent they own the relevant rights. CC0 does
not relicense separately installed dependencies or any third-party material.
Check any reused contributions before applying the public-domain dedication.

PySide6 is an external dependency with its own license terms. The project uses
TiddlyWiki-compatible data conventions but does not claim to contain the
TiddlyWiki product itself.

## How the experiment fits together

- **Personal Wiki:** collect and navigate knowledge locally.
- **Forms and arguments:** capture project scope, alternatives, decisions,
  causes, progress, and reasons; use Toulmin fields for the argument within
  each supported form.
- **Projects and contracts:** track work while iteratively making post metadata
  more structured.
- **PVS-01 bridge:** inspect externally governed project/versioning state and
  expose status differences for human reconciliation.

The result is a personal, experimental environment for organizing both **what
changed** and **why**. See [README.md](README.md) and the
[user manual](docs/USER_MANUAL.md) for a feature-level description.
